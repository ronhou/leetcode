# 模拟
线性扫描，先获取第一个递增序列，然后继续扫描获取第二个递增序列。第二个递增序列的长度即答案。

```C++
class Solution
{
public:
	int minimumRightShifts(std::vector<int> &nums)
	{
		int i = 1;
		while (i < nums.size() && nums[i] > nums[i - 1])
			i++;
		if (i >= nums.size())
			return 0;
		int j = i + 1;
		while (j < nums.size() && nums[j] > nums[j - 1])
			j++;
		if (j < nums.size() || nums[j - 1] > nums[0])
			return -1;
		return j - i;
	}
};
```