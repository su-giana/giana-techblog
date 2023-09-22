# 1. List 이해하기
> [!NOTE] List란?
> 일반적인 Linked List로 1장에서는 threads를 저장하기 위해서 사용되었다

실제 [[thread-kor|스레드 문서]]를 읽어보면, list_elem이 설치된 것을 볼 수 있다.
```C
/* 해당 포인터가 가리키는 원소를 실제 안쪽에 구현된 list_elem으로 변환해줌 */

#define list_entry(LIST_ELEM, STRUCT, MEMBER)           \
	((STRUCT *) ((uint8_t *) &(LIST_ELEM)->next     \
		- offsetof (STRUCT, MEMBER.next)))

/* 리스트 원소 */
struct list_elem {
	struct list_elem *prev;     /* 이전 원소 */
	struct list_elem *next;     /* 다음 원소 */
};

/* 리스트 구조체 */
struct list {
	struct list_elem head;      /* 리스트의 Head */
	struct list_elem tail;      /* 리스트의 Tail. */
};
```

---
# 자주 사용되는 함수
![[Screenshot 2023-09-22 at 11.48.07 PM.png]]

```c
// 가장 앞에 있는 원소 제거
struct list_elem *
list_pop_front (struct list *list) {
	struct list_elem *front = list_front (list);
	list_remove (front);
	return front;
}

// 가장 앞의 원소 반환 
struct list_elem *
list_front (struct list *list) {
	ASSERT (!list_empty (list));
	return list->head.next;
}

// 원소를 리스트에 정렬하여 넣음
// ex) 1 -> 3 -> 4 + 2 => 1 -> 2 -> 3-> 4
void
list_insert_ordered (struct list *list, struct list_elem *elem,
		list_less_func *less, void *aux) {
	struct list_elem *e;

	ASSERT (list != NULL);
	ASSERT (elem != NULL);
	ASSERT (less != NULL);

	for (e = list_begin (list); e != list_end (list); e = list_next (e))
		if (less (elem, e, aux))
			break;
	return list_insert (e, elem);
}
```

