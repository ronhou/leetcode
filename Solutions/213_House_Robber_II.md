# 动态规划
此题属于 [198. House Robber](https://leetcode.cn/problems/house-robber/) 的进阶版，可以多使用两个状态来标记是否选择第0间房子。

```C++
class Solution
{
public:
	int rob(std::vector<int> &nums)
	{
		if (nums.size() == 1)
			return nums[0];

		/**
		 * rob[i][0]表示不选i，也不选0
		 * rob[i][1]表示不选i，但是选0
		 * rob[i][2]表示选i，但是不选0
		 * rob[i][3]表示选i，也选0
		 */
		const int sz = nums.size();
		std::vector<std::vector<int>> robs(sz, std::vector<int>(4, -1));
		robs[0][0] = 0;
		robs[0][3] = nums[0];
		for (int i = 1; i < sz; i++)
		{
			robs[i][0] = std::max(robs[i - 1][0], robs[i - 1][2]);
			robs[i][1] = std::max(robs[i - 1][1], robs[i - 1][3]);
			if (robs[i - 1][0] != -1)
				robs[i][2] = robs[i - 1][0] + nums[i];
			if (robs[i - 1][1])
				robs[i][3] = robs[i - 1][1] + nums[i];
		}
		return std::max(robs[sz - 1][0], std::max(robs[sz - 1][1], robs[sz - 1][2]));
	}
};
```