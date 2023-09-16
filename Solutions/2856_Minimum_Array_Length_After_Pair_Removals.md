# 贪心
计算数字出现重复最多的次数`mostNumCount`，如果剩下的数字的个数大于等于`mostNumCount`，那么所有数字（除非个数是奇数，会剩下一个）都可以被通过删除数对而消除。否则的话，多余的个数即删剩下的最小长度。

证明：待大牛们证明。

```C++ [贪心]
class Solution
{
public:
	int minLengthAfterRemovals(std::vector<int> &nums)
	{
		int mostNumCount = 1, count = 1;
		for (int i = 1; i < nums.size(); i++)
		{
			if (nums[i] != nums[i - 1])
			{
				count = 1;
			}
			else
			{
				++count;
				mostNumCount = std::max(mostNumCount, count);
			}
		}
		if (nums.size() - mostNumCount >= mostNumCount)
			return nums.size() % 2;
		else
			return mostNumCount - (nums.size() - mostNumCount);
	}
};
```