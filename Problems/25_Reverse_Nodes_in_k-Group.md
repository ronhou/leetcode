# 复用上一题（Problem92）reverseListBetween
```C++
ListNode *reverseListByKGroup(ListNode *head, int k)
{
	ListNode root(0, head), *rootPtr = &root, *last = rootPtr;
	while (true)
	{
		ListNode *leftPtr = last->next, *rightPtr = last->next;
		int n = k;
		while (rightPtr != nullptr && (--n))
			rightPtr = rightPtr->next;
		if (rightPtr == nullptr)
			break;
		last->next = reverseListBetween(last->next, 1, k);

		n = k;
		while (n--)
			last = last->next;
	}
	return root.next;
}
```