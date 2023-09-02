# 辅助数组/辅助指针
+ 辅助数组：使用左右两个数组，线扫保存小于x的部分和大于等于x的部分，然后链接链表和左右两部分。
+ 辅助指针：使用辅助指针分别指向左右两部分的头尾，依次将结点加入左右两部分的尾指针后面，然后链接左右两部分。
```C++ [辅助数组]
ListNode *partitionListByVal(ListNode *head, int x)
{
	std::vector<ListNode *> leftPart, rightPart;
	ListNode *ptr = head;
	while (ptr != nullptr)
	{
		if (ptr->val < x)
			leftPart.push_back(ptr);
		else
			rightPart.push_back(ptr);
		ptr = ptr->next;
	}
	if (!leftPart.empty())
	{
		for (int i = 0; i < leftPart.size() - 1; i++)
			leftPart[i]->next = leftPart[i + 1];
		leftPart.back()->next = !rightPart.empty() ? rightPart[0] : nullptr;
	}
	if (!rightPart.empty())
	{
		for (int i = 0; i < rightPart.size() - 1; i++)
			rightPart[i]->next = rightPart[i + 1];
		rightPart.back()->next = nullptr;
	}
	return !leftPart.empty() ? leftPart[0] : (!rightPart.empty() ? rightPart[0] : nullptr);
}
```
```C++ [辅助指针]
ListNode *partitionListByVal(ListNode *head, int x)
{
	ListNode *leftHead = nullptr, *leftTail = nullptr, *rightHead = nullptr, *rightTail = nullptr;
	for (ListNode *it = head; it != nullptr;)
	{
		ListNode *next = it->next;
		if (it->val < x)
		{
			if (leftHead == nullptr)
			{
				leftHead = leftTail = it;
			}
			else
			{
				leftTail->next = it;
				leftTail = it;
			}
		}
		else
		{
			if (rightHead == nullptr)
			{
				rightHead = rightTail = it;
			}
			else
			{
				rightTail->next = it;
				rightTail = it;
			}
		}
		it = next;
	}
	if (leftTail != nullptr)
		leftTail->next = rightHead;
	if (rightTail != nullptr)
		rightTail->next = nullptr;
	return leftHead ? leftHead : rightHead;
}
```