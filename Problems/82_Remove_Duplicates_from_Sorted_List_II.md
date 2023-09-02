# 哈希表+一次遍历
```C++ [哈希表计数+一次遍历]
ListNode *removeDuplicateNodes(ListNode *head)
{
	std::unordered_map<int, int> valueCounts;
	ListNode *ptr = head;
	while (ptr != nullptr)
	{
		++valueCounts[ptr->val];
		ptr = ptr->next;
	}

	ListNode root(0, head);
	ptr = &root;
	while (ptr->next != nullptr)
	{
		if (valueCounts[ptr->next->val] > 1)
		{
			ptr->next = ptr->next->next;
		}
		else
		{
			ptr = ptr->next;
		}
	}
	return root.next;
}
```
```C++ [链表已有序+一次遍历]
ListNode *removeDuplicateNodes(ListNode *head)
{
	ListNode root(0, head), *prevPtr = &root, *ptr = head;
	while (ptr != nullptr)
	{
		while (ptr->next != nullptr && ptr->next->val == ptr->val)
			ptr = ptr->next;
		if (ptr != prevPtr->next)
			prevPtr->next = ptr->next;
		else
			prevPtr = ptr;
		ptr = ptr->next;
	}
	return root.next;
}
```