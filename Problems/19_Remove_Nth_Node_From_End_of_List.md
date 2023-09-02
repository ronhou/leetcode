# 拼接被删除结点的上一个和下一个
将上一个结点的next指向下一个结点：`prev->next = prev->next->next`
```C++
ListNode *removeLastNthNode(ListNode *head, int n)
{
	int count = 0;
	ListNode *ptr = head;
	while (ptr != nullptr)
	{
		++count;
		ptr = ptr->next;
	}

	ListNode root(0, head);
	ptr = &root;
	n = count - n;
	while (n--)
	{
		ptr = ptr->next;
	}
	ptr->next = ptr->next->next;
	return root.next;
}
```