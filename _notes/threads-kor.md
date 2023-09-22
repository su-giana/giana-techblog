# Alarm Clock
## 0. 전체 소스코드
```c
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
- timer의 행동 규약은 [[timer-kor|여기]]를 참고
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
