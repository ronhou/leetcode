# 快慢指针
```
class Solution
{
public:
	bool hasCycle(ListNode *head)
	{
		ListNode *slow = head, *fast = head != NULL ? head->next : NULL;
		while (fast != NULL && slow != fast)
		{
			fast = fast->next;
			if (fast != NULL) {
				fast = fast->next;
			}
			slow = slow->next;
		}
		return fast != NULL;
	}
};
```