---
sticker: emoji//26c8-fe0f
---
# 스레드 이해하기
> [!NOTE] 스레드란?
> 프로세스 내에서 실행되는 최소 실행 단위

```c
struct thread {
	tid_t tid;                          
	enum thread_status status;          
	char name[16];                      
	int priority;                       

	/* Shared between thread.c and synch.c. */
	struct list_elem elem;              /* List element. */

#ifdef USERPROG
	/* Owned by userprog/process.c. */
	uint64_t *pml4;                     /* Page map level 4 */
#endif
#ifdef VM
	/* Table for whole virtual memory owned by thread. */
	struct supplemental_page_table spt;
#endif

	/* Owned by thread.c. */
	struct intr_frame tf;               /* Information for switching */
	unsigned magic;                     /* Detects stack overflow. */
};

```
---
# thread 함수의 이해
## thread의 초기화
```c
// 동작하는 스레드 반환
// rsp 위치를 반올림하여 페이지 시작에 전달
#define running_thread() ((struct thread *) (pg_round_down (rrsp ())))

void
thread_init (void) {
	ASSERT (intr_get_level () == INTR_OFF);

	/* Reload the temporal gdt for the kernel
	 * This gdt does not include the user context.
	 * The kernel will rebuild the gdt with user context, in gdt_init (). */
	struct desc_ptr gdt_ds = {
		.size = sizeof (gdt) - 1,
		.address = (uint64_t) gdt
	};
	lgdt (&gdt_ds);

	/* Init the globla thread context */
	lock_init (&tid_lock);
	list_init (&ready_list);
	list_init (&destruction_req);
	list_init (&block_list);

	/* Set up a thread structure for the running thread. */
	initial_thread = running_thread ();
	init_thread (initial_thread, "main", PRI_DEFAULT);  // 메인 스레드
	initial_thread->status = THREAD_RUNNING;
	initial_thread->tid = allocate_tid ();
}

static void
init_thread (struct thread *t, const char *name, int priority) {
	ASSERT (t != NULL);
	ASSERT (PRI_MIN <= priority && priority <= PRI_MAX);
	ASSERT (name != NULL);

	memset (t, 0, sizeof *t);
	t->status = THREAD_BLOCKED;
	strlcpy (t->name, name, sizeof t->name);
	t->tf.rsp = (uint64_t) t + PGSIZE - sizeof (void *);    // rsp == register stack pointer
	t->priority = priority;
	t->magic = THREAD_MAGIC;
}
```
> [!NOTE] What is GDT?
> GDT stands for Global Descriptor Table. It is a data structure used in the x86 architecture to manage memory segmentation. the use of the Global Descriptor Table is often replaced or supplemented by the Long Mode Global Descriptor Table (LGDT), which is part of the x86-64 architecture.

### Current Thread
```c
/* Returns the running thread.
 * Read the CPU's stack pointer `rsp', and then round that
 * down to the start of a page.  Since `struct thread' is
 * always at the beginning of a page and the stack pointer is
 * somewhere in the middle, this locates the curent thread. */
#define running_thread() ((struct thread *) (pg_round_down (rrsp ())))
```
## thread가 tick을 재는 방법
```c
/* Called by the timer interrupt handler at each timer tick.
   Thus, this function runs in an external interrupt context. */
void
thread_tick (void) {
	struct thread *t = thread_current ();

	/* Update statistics. */
	if (t == idle_thread)
		idle_ticks++;
#ifdef USERPROG
	else if (t->pml4 != NULL)
		user_ticks++;
#endif
	else
		kernel_ticks++;

// time_slice가 될 때까지 CPU 사용권을 다른 스레드에 넘김
	if (++thread_ticks >= TIME_SLICE)
		intr_yield_on_return ();
}
```