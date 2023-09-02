# 回溯逆转链表，模拟拼接链表
```C++
void reverseFromHeadToLast(ListNode *head, ListNode *last)
{
	if (head == last)
		return;
	ListNode *next = head->next;
	reverseFromHeadToLast(next, last);
	next->next = head;
}

ListNode *reverseListBetween(ListNode *head, int left, int right)
{
	ListNode root(0, head), *pointer = &root;
	right -= left;
	while (--left)
		pointer = pointer->next;
	ListNode *prev = pointer;
	pointer = pointer->next;
	while (right--)
		pointer = pointer->next;
	ListNode *last = pointer, *next = pointer->next;
	reverseFromHeadToLast(prev->next, last);
	prev->next->next = next;
	prev->next = last;
	return root.next;
}
```