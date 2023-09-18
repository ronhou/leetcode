# 归并排序
1. 使用快慢指针找到链表的中间结点
2. 以中间结点为划分点，将链表划分为左右两部分
3. 使用分治的思想，递归处理对左右两部分分别进行归并排序
4. 再将排序好的左右两个链表进行合并，返回合并后的首结点

```C++ [空间复杂度O(logn)]
class Solution
{
public:
	ListNode *sortList(ListNode *head)
	{
		return mergeSortList(head, nullptr);
	}

	ListNode *mergeSortList(ListNode *head, ListNode *tail)
	{
		if (head == nullptr)
			return head;
		if (head->next == tail)
		{
			head->next = nullptr;
			return head;
		}

		ListNode *fast = head, *slow = head;
		while (fast != tail)
		{
			slow = slow->next;
			fast = fast->next;
			if (fast != tail)
				fast = fast->next;
		}
		head = mergeSortList(head, slow);
		slow = mergeSortList(slow, tail)
		return mergeSortedList(head, slow);
	}

	ListNode *mergeSortedList(ListNode *head1, ListNode *head2)
	{
		ListNode headNode, *tail = &headNode;
		while (head1 != nullptr && head2 != nullptr)
		{
			if (head1->val < head2->val)
			{
				tail->next = head1;
				head1 = head1->next;
			}
			else
			{
				tail->next = head2;
				head2 = head2->next;
			}
			tail = tail->next;
		}
		tail->next = head1 != nullptr ? head1 : head2;
		return headNode.next;
	}
};
```

## [力扣官网题解](https://leetcode.cn/problems/sort-list/solutions/492301/pai-xu-lian-biao-by-leetcode-solution/?envType=study-plan-v2&envId=top-interview-150)
学习力扣官方题解方法二，依旧是归并排序的思想，但是不是采用递归回溯，而是将链表依次划分成长度为1，2，4...的片段进行归并排序
```C++
ListNode *sortList(ListNode *head)
{
	if (head == nullptr)
		return nullptr;

	int length = 0;
	ListNode *curr = head;
	while (curr != nullptr)
	{
		++length;
		curr = curr->next;
	}

	ListNode dummyHead(0, head), *last = &dummyHead;
	ListNode *head1 = nullptr, *head2 = nullptr, *prev = nullptr;
	for (int subLen = 1; subLen < length; subLen <<= 1)
	{
		last = &dummyHead;
		curr = dummyHead.next;
		while (curr != nullptr)
		{
			prev = last;
			head1 = curr;
			for (int i = 0; i < subLen && curr != nullptr; i++)
			{
				prev = curr;
				curr = curr->next;
			}
			prev->next = nullptr;

			head2 = curr;
			for (int i = 0; i < subLen && curr != nullptr; i++)
			{
				prev = curr;
				curr = curr->next;
			}
			prev->next = nullptr;

			last->next = mergeSortedList(head1, head2);
			while (last->next != nullptr)
				last = last->next;
		}
	}
	return dummyHead.next;
}
```