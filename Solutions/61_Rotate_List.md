```C++
ListNode *rotateListRight(ListNode *head, int k)
{
	if (head == nullptr)
		return head;

	int sz = 0;
	ListNode *node = head, *prev = nullptr;
	while (node != nullptr)
	{
		++sz;
		prev = node;
		node = node->next;
	}

	k = k % sz;
	if (k == 0)
		return head;

	k = sz - k;
	// 闭合为环
	prev->next = head;
	// 寻找新的结束结点和开始结点
	node = head;
	while (--k)
		node = node->next;
	// node->next为新的开始结点
	head = node->next;
	// node为新的结束结点
	node->next = nullptr;
	return head;
}
```