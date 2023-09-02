# 链表指针
```C++
ListNode *addTwoListNodeValues(ListNode *l1, ListNode *l2)
{
	ListNode head, *pointer = &head;
	int carry = 0;
	while (l1 != nullptr || l2 != nullptr)
	{
		int sum = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + carry;
		pointer->next = new ListNode(sum % 10);
		if (l1)
			l1 = l1->next;
		if (l2)
			l2 = l2->next;
		pointer = pointer->next;
		carry = sum / 10;
	}
	if (carry)
		pointer->next = new ListNode(carry);
	return head.next;
}

```