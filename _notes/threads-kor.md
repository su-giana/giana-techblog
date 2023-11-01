# Alarm Clock
## 0. 전체 소스코드
```c
--- a/devices/timer.c
+++ b/devices/timer.c
@@ -87,14 +87,31 @@ timer_elapsed (int64_t then) {
 	return timer_ticks () - then;
 }
 
+static bool compare_tick (const struct list_elem *A,
+		const struct list_elem *B, void *aux UNUSED) {
+	const struct thread *threadA = list_entry (A, struct thread, elem);
+	const struct thread *threadB = list_entry (B, struct thread, elem);
+	return threadA->ticks < threadB->ticks;
+}
+
 /* Suspends execution for approximately TICKS timer ticks. */
 void
 timer_sleep (int64_t ticks) {
 	int64_t start = timer_ticks ();
 
 	ASSERT (intr_get_level () == INTR_ON);
+	/*
 	while (timer_elapsed (start) < ticks)
 		thread_yield ();
+	*/
+	/* Solution */
+	struct thread *th = thread_current ();
+	enum intr_level old_level = intr_disable ();
+	th->ticks = ticks + start;
+	list_insert_ordered (&block_list, &th->elem, compare_tick, NULL);
+	thread_block ();
+	intr_set_level(old_level);
+
 }
 
 /* Suspends execution for approximately MS milliseconds. */
@@ -126,6 +143,18 @@ static void
 timer_interrupt (struct intr_frame *args UNUSED) {
 	ticks++;
 	thread_tick ();
+
+	/* Solution */
+	struct thread *th;
+
+	while (!list_empty (&block_list)) {
+		th = list_entry (list_front (&block_list), struct thread, elem);
+		if (ticks >= th->ticks) {
+			list_pop_front (&block_list);
+			thread_unblock (th);
+		} else
+			break;
+	}
 }

--- a/threads/thread.c
+++ b/threads/thread.c
@@ -10,6 +10,7 @@
 #include "threads/palloc.h"
 #include "threads/synch.h"
 #include "threads/vaddr.h"
+#include "devices/timer.h"
 #include "intrinsic.h"
 #ifdef USERPROG
 #include "userprog/process.h"
@@ -40,6 +41,11 @@ static struct lock tid_lock;
 /* Thread destruction requests */
 static struct list destruction_req;
 
+/* Solution */
+/* List of blocked processes */
+struct list block_list;
+FP load_avg;
+
 /* Statistics. */
 static long long idle_ticks;    /* # of timer ticks spent idle. */
 static long long kernel_ticks;  /* # of timer ticks in kernel threads. */
@@ -109,6 +115,11 @@ thread_init (void) {
 	lock_init (&tid_lock);
 	list_init (&ready_list);
 	list_init (&destruction_req);
+	/* Solution */
+	list_init (&block_list);
+	if (thread_mlfqs)
+		load_avg = 0;
+	/* Solution done. */
 
 	/* Set up a thread structure for the running thread. */
 	initial_thread = running_thread ();
@@ -133,6 +144,43 @@ thread_start (void) {
 	sema_down (&idle_started);
 }

```
---
## 1. 리스트 생성하기
- 리스트에 대한 이해는 [[list-kor|여기]]

```c
+struct list block_list;
+FP load_avg;
+
 /* Statistics. */
 static long long idle_ticks;    /* # of timer ticks spent idle. */
 static long long kernel_ticks;  /* # of timer ticks in kernel threads. */
@@ -109,6 +115,11 @@ thread_init (void) {
 	lock_init (&tid_lock);
 	+	list_init (&block_list);
```
> [!NOTE] 리스트의 용도
> 블록된 프로세스들의 리스트 만들기 -> 이후 스레드 넣고 빼는 데에 사용

## 2. 비교 로직 만들기
- thread의 로직은 [[thread-kor|여기]]를 참고

```c
+static bool compare_tick (const struct list_elem *A,
+		const struct list_elem *B, void *aux UNUSED) {
+	const struct thread *threadA = list_entry (A, struct thread, elem);
+	const struct thread *threadB = list_entry (B, struct thread, elem);
+	return threadA->ticks < threadB->ticks;
+}
+
```
> [!NOTE] 비교 로직
> 해당 프로세스의 종료 시간대를 비교하여 정렬에 사용할 수 있는 함수 생성

## 3. 인터럽트 행동 규약

```c
timer_interrupt (struct intr_frame *args UNUSED) {
 	ticks++;
 	thread_tick ();
+
+	/* Solution */
+	struct thread *th;
+
+	while (!list_empty (&block_list)) {
+		th = list_entry (list_front (&block_list), struct thread, elem);
+		if (ticks >= th->ticks) {
+			list_pop_front (&block_list);
+			thread_unblock (th);
+		} else
+			break;
+	}
 }
```
> [!NOTE] 행동 규약
> 타이머 인터럽트가 발생하면 블록 리스트 중에 시작 시간이 초과하거나 같은 것을 실행시킨다
> (인터럽트는 timer_tick 시간만큼 실행 후 스레드들을 검사한다)

## 4. 타이머 time_sleep 함수 변경
```c
+	struct thread *th = thread_current ();
+	enum intr_level old_level = intr_disable ();
+	th->ticks = ticks + start;
+	list_insert_ordered (&block_list, &th->elem, compare_tick, NULL);
+	thread_block ();
+	intr_set_level(old_level);
+
```
> [!NOTE] 왜 인터럽트를 꺼야하나요?
> 인터럽트에 활용되는 8254 타이머가 선점형이므로, 타이머 중에 인터럽트가 발생할 수 있기 때문입니다

> [!NOTE] 동작 원리
> 현재 스레드의 종료 시점을 타이머 종료 시점으로 변경한 후 실행할 프로세스 리스트에 해당 스레드를 삽입합니다. 스레드를 블록한 후 스레드가 언록되고 해제되면 인터럽트 레벨을 다시 되돌립니다

---
# Priority Scheduling

```c
--- /dev/null
+++ b/include/threads/fixed-point.h
@@ -0,0 +1,10 @@
+#ifndef FIXED_POINT_H
+#define FIXED_POINT_H
+#define fp_fr 14
+#define fp_f (1 << 14)
+#define FP(x) ((x) << 14)
+#define FP2INT(x) ((x) >> 14)
+#define MUL(x, y) (((int64_t)(x)) * (y) / (fp_f))
+#define DIV(x, y) (((int64_t)(x)) * (fp_f) / (y))
+typedef int32_t FP;
+#endif
diff --git a/include/threads/synch.h b/include/threads/synch.h
index 3d089fd..60a92ac 100644
--- a/include/threads/synch.h
+++ b/include/threads/synch.h
@@ -18,6 +18,10 @@ void sema_self_test (void);
 
 /* Lock. */
 struct lock {
+	/* Solutions */
+	struct list_elem elem;
+	struct list waiters;
+	/* Solutions done. */
 	struct thread *holder;      /* Thread holding lock (for debugging). */
 	struct semaphore semaphore; /* Binary semaphore controlling access. */
 };
diff --git a/include/threads/thread.h b/include/threads/thread.h
index 33b46e6..fcb4d9a 100644
--- a/include/threads/thread.h
+++ b/include/threads/thread.h
@@ -5,6 +5,7 @@
 #include <list.h>
 #include <stdint.h>
 #include "threads/interrupt.h"
+#include "threads/fixed-point.h"
 #ifdef VM
 #include "vm/vm.h"
 #endif
@@ -95,6 +96,19 @@ struct thread {
 	/* Shared between thread.c and synch.c. */
 	struct list_elem elem;              /* List element. */
 
+	/* Solution */
+	int64_t ticks;                      /* Saved ticks */
+	int effective_priority;             /* Effective Priority */
+	struct list_elem lock_elem;         /* for waiters in struct lock */
+	struct list locks;                  /* List of locks thread hold */
+
+	struct lock *waiting_lock;
+	struct thread *donator;
+	struct thread *donatee;
+	int nice;
+	FP recent_cpu;
+	/* Solution done. */
+
 #ifdef USERPROG
 	/* Owned by userprog/process.c. */
 	uint64_t *pml4;                     /* Page map level 4 */
@@ -114,6 +128,13 @@ struct thread {
    Controlled by kernel command-line option "-o mlfqs". */
 extern bool thread_mlfqs;
 
+/* Solution */
+struct list block_list;
+bool compare_priority (const struct list_elem *A,
+		const struct list_elem *B, void *aux UNUSED);
+
+/* Solution done. */
+
 void thread_init (void);
 void thread_start (void);
 
diff --git a/threads/synch.c b/threads/synch.c
index 8ca3230..7f13cb8 100644
--- a/threads/synch.c
+++ b/threads/synch.c
@@ -109,10 +109,26 @@ sema_up (struct semaphore *sema) {
 	ASSERT (sema != NULL);
 
 	old_level = intr_disable ();
+	/*
 	if (!list_empty (&sema->waiters))
 		thread_unblock (list_entry (list_pop_front (&sema->waiters),
 					struct thread, elem));
+	*/
+	/* Solution */
+	struct thread *th = NULL;
+	if (!list_empty (&sema->waiters)) {
+		struct list_elem *elem =
+			list_max (&sema->waiters, compare_priority, NULL);
+		th = list_entry(elem, struct thread, elem);
+		list_remove (elem);
+		thread_unblock (th);
+	}
+	/* Solution done. */
 	sema->value++;
+	if (!intr_context () && th &&
+			th->effective_priority > thread_current ()->effective_priority)
+		thread_yield();
+
 	intr_set_level (old_level);
 }
 
@@ -150,7 +166,7 @@ sema_test_helper (void *sema_) {
 		sema_up (&sema[1]);
 	}
 }
-
+
 /* Initializes LOCK.  A lock can be held by at most a single
    thread at any given time.  Our locks are not "recursive", that
    is, it is an error for the thread currently holding a lock to
@@ -170,10 +186,60 @@ void
 lock_init (struct lock *lock) {
 	ASSERT (lock != NULL);
 
+	/* Solution */
+	list_init (&lock->waiters);
+	/* Solution done. */
+
 	lock->holder = NULL;
 	sema_init (&lock->semaphore, 1);
 }
 
+/* Solution */
+/* Compare Priority of Threads */
+static bool
+compare_priority_in_lock (const struct list_elem *A,
+		const struct list_elem *B, void *aux UNUSED) {
+    const struct thread *threadA = list_entry (A, struct thread, lock_elem);
+    const struct thread *threadB = list_entry (B, struct thread, lock_elem);
+    return threadA->effective_priority < threadB->effective_priority;
+}
+
+/* get maximum priority between holding locks' waiters */
+static bool
+compare_priority_in_locks (const struct list_elem *A,
+		const struct list_elem *B, void *aux UNUSED) {
+	struct lock *lockA = list_entry (A, struct lock, elem);
+	struct lock *lockB = list_entry (B, struct lock, elem);
+
+	struct thread *priorityA = list_entry (list_max (&lockA->waiters, compare_priority_in_lock, NULL), struct thread, lock_elem);
+	struct thread *priorityB = list_entry (list_max (&lockB->waiters, compare_priority_in_lock, NULL), struct thread, lock_elem);
+    return priorityA < priorityB;
+}
+
+/* donate priority to holder to max priority of waiters */
+static void
+donate_effective_priority (struct thread *holder) {
+	holder->effective_priority = holder->priority;
+	if (!list_empty (&holder->locks)) {
+		struct lock *l = list_entry (
+				list_max (&holder->locks, compare_priority_in_locks, NULL),
+				struct lock, elem);
+		if (list_empty(&l->waiters))
+			return;
+		struct thread *t = list_entry (
+				list_max (&l->waiters, compare_priority_in_lock, NULL),
+				struct thread, lock_elem);
+
+		if (t && t->effective_priority > holder->effective_priority) {
+			holder->effective_priority = t->effective_priority;
+			struct lock *l = holder->waiting_lock;
+			if (l && l->holder)
+				donate_effective_priority (l->holder);
+		}
+	}
+}
+/* Solution done. */
+
 /* Acquires LOCK, sleeping until it becomes available if
    necessary.  The lock must not already be held by the current
    thread.
@@ -188,8 +254,36 @@ lock_acquire (struct lock *lock) {
 	ASSERT (!intr_context ());
 	ASSERT (!lock_held_by_current_thread (lock));
 
+	/*
 	sema_down (&lock->semaphore);
 	lock->holder = thread_current ();
+	*/
+
+	/* Solution */
+	struct thread *cur = thread_current ();
+	struct thread *holder = lock->holder;
+	if(thread_mlfqs) {
+		sema_down (&lock->semaphore);
+		lock->holder = cur;
+		return;
+	}
+
+	if (holder) {
+		/* insert current thread into lock */
+		list_push_back (&lock->waiters, &cur->lock_elem);
+		cur->waiting_lock = lock;
+		donate_effective_priority (holder);
+	}
+	sema_down (&lock->semaphore);
+
+	/* remove current thread from waiting list */
+	if (holder)
+		list_remove (&cur->lock_elem);
+	cur->waiting_lock = NULL;
+	/* insert current lock into holding locks list */
+	list_push_back (&cur->locks, &lock->elem);
+	lock->holder = cur;
+	/* Solution done. */
 }
 
 /* Tries to acquires LOCK and returns true if successful or false
@@ -222,7 +316,18 @@ lock_release (struct lock *lock) {
 	ASSERT (lock != NULL);
 	ASSERT (lock_held_by_current_thread (lock));
 
+	//lock->holder = NULL;
+	/* Solution */
+	if(thread_mlfqs) {
+		lock->holder = NULL;
+		sema_up (&lock->semaphore);
+		return;
+	}
+	struct thread *holder = lock->holder;
+	list_remove (&lock->elem);      /* remove lock from holding list */
 	lock->holder = NULL;
+	donate_effective_priority (holder); /* recalculate priority */
+	/* Solution done. */
 	sema_up (&lock->semaphore);
 }
 
@@ -288,6 +393,28 @@ cond_wait (struct condition *cond, struct lock *lock) {
 	lock_acquire (lock);
 }
 
+/* Solution */
+/* get semaphore which has maximum eff_priority */
+static bool
+compare_priority_cond (const struct list_elem *A,
+		const struct list_elem *B, void *aux UNUSED) {
+	struct semaphore *semaphoreA =
+		&list_entry (A, struct semaphore_elem, elem)->semaphore;
+	struct semaphore *semaphoreB =
+		&list_entry (B, struct semaphore_elem, elem)->semaphore;
+	const struct thread *threadA;
+	const struct thread *threadB;
+
+	threadA = list_entry (
+			list_max (&semaphoreA->waiters, compare_priority, NULL),
+			struct thread, elem);
+	threadB = list_entry (
+			list_max (&semaphoreB->waiters, compare_priority, NULL),
+			struct thread, elem);
+	return threadA->priority < threadB->priority;
+}
+/* Solution done. */
+
 /* If any threads are waiting on COND (protected by LOCK), then
    this function signals one of them to wake up from its wait.
    LOCK must be held before calling this function.
@@ -302,9 +429,23 @@ cond_signal (struct condition *cond, struct lock *lock UNUSED) {
 	ASSERT (!intr_context ());
 	ASSERT (lock_held_by_current_thread (lock));
 
+	/*
 	if (!list_empty (&cond->waiters))
 		sema_up (&list_entry (list_pop_front (&cond->waiters),
 					struct semaphore_elem, elem)->semaphore);
+	*/
+	/* Solution */
+	struct list_elem *elem;
+	struct semaphore *sema;
+	if (!list_empty (&cond->waiters)) {
+		/* sema_up according to comp_priority_cond */
+		elem = list_max (&cond->waiters, compare_priority_cond, NULL);
+		sema = &list_entry (elem, struct semaphore_elem, elem)->semaphore;
+
+		list_remove (elem);
+		sema_up (sema);
+	}
+	/* Solution done. */
 }
```

## 1. 현재 CPU를 파악하기 위한 Fixed Point 구조체 만들기
```c
--- /dev/null
+++ b/include/threads/fixed-point.h
@@ -0,0 +1,10 @@
+#ifndef FIXED_POINT_H
+#define FIXED_POINT_H
+#define fp_fr 14
+#define fp_f (1 << 14)
+#define FP(x) ((x) << 14)
+#define FP2INT(x) ((x) >> 14)
+#define MUL(x, y) (((int64_t)(x)) * (y) / (fp_f))
+#define DIV(x, y) (((int64_t)(x)) * (fp_f) / (y))
+typedef int32_t FP;
+#endif
```
