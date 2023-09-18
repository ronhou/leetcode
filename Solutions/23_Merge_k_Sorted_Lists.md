## 分治合并
因为合并结果和合并顺序无关，因此可以采取分治的方式，划分成更细的粒度进行合并，使用递归将问题转换为粒度更小的子问题。
```C++ [分治]
class Solution
{
public:
	ListNode *mergeKLists(std::vector<ListNode *> &lists)
	{
		if (lists.size() == 0)
			return nullptr;
		return mergeSortedLists(lists, 0, lists.size() - 1);
	}

	ListNode *mergeSortedLists(const std::vector<ListNode *> &lists, int begin, int end)
	{
		if (begin >= end)
			return lists[begin];

		int center = (begin + end) / 2;
		ListNode *head1 = mergeSortedLists(lists, begin, center);
		ListNode *head2 = mergeSortedLists(lists, center + 1, end);
		return mergeSortedLists(head1, head2);
	}

	ListNode *mergeSortedLists(ListNode *head1, ListNode *head2)
	{
		ListNode dummyNode, *tail = &dummyNode;
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
		return dummyNode.next;
	}
};
```

## 优先队列
还有另外一种方式，就是将所有链表结点保存到优先队列中，每次取值最小的结点。