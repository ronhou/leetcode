# 链表指针
```C++
ListNode *mergeTwoLists(ListNode *list1, ListNode *list2)
{
	ListNode head, *pointer = &head;
	while (list1 != nullptr && list2 != nullptr)
	{
		if (list1->val < list2->val)
		{
			pointer->next = list1;
			list1 = list1->next;
		}
		else
		{
			pointer->next = list2;
			list2 = list2->next;
		}
		pointer = pointer->next;
	}
	pointer->next = list1 != nullptr ? list1 : list2;
	return head.next;
}
```