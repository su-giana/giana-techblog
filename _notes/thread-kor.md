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

## thread의 tick
```c
static void
schedule (void) {
	struct thread *curr = running_thread ();
	struct thread *next = next_thread_to_run ();

	ASSERT (intr_get_level () == INTR_OFF);
	ASSERT (curr->status != THREAD_RUNNING);
	ASSERT (is_thread (next));
	/* Mark us as running. */
	next->status = THREAD_RUNNING;

	/* Start new time slice. */
	thread_ticks = 0;

#ifdef USERPROG
	/* Activate the new address space. */
	process_activate (next);
#endif

	if (curr != next) {
		/* If the thread we switched from is dying, destroy its struct
		   thread. This must happen late so that thread_exit() doesn't
		   pull out the rug under itself.
		   We just queuing the page free reqeust here because the page is
		   currently used by the stack.
		   The real destruction logic will be called at the beginning of the
		   schedule(). */
		if (curr && curr->status == THREAD_DYING && curr != initial_thread) {
			ASSERT (curr != next);
			list_push_back (&destruction_req, &curr->elem);
		}

		/* Before switching the thread, we first save the information
		 * of current running. */
		thread_launch (next);
	}
}

// 타이머 틱마다 인터럽트 핸들러에 의하여 호출
void
thread_tick (void) {
	struct thread *t = thread_current ();

	/* Update statistics. */
	if (t == idle_thread)  // 아무것도 동작하지 않는 상태
		idle_ticks++;
#ifdef USERPROG
	else if (t->pml4 != NULL)  // 사용자 레벨 코드의 사용시간 측정
		user_ticks++;
#endif
	else    // 커널 레벨 코드의 사용시간 측정
		kernel_ticks++;

	/* Enforce preemption. */
	if (++thread_ticks >= TIME_SLICE)
		intr_yield_on_return ();
}
```