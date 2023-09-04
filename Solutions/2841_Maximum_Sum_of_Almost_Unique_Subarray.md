> 第 112 场双周赛，第3题（#2841）：  
> [2841. Maximum Sum of Almost Unique Subarray](https://leetcode.cn/contest/biweekly-contest-112/problems/maximum-sum-of-almost-unique-subarray/)

# 滑动窗口+哈希表
固定窗口大小为k的滑动窗口，使用哈希表统计窗口内的数字和它们出现的次数。
```C++
class Solution
{
public:
	long long maxSum(std::vector<int> &nums, int m, int k)
	{
		if (nums.size() < k)
			return 0;

		long long sum = 0;
		std::unordered_map<int, int> numCounts;
		for (int i = 0; i < k; i++)
		{
			sum += nums[i];
			++numCounts[nums[i]];
		}

		long long maximumSum = numCounts.size() >= m ? sum : 0;
		for (int i = k; i < nums.size(); i++)
		{
			if (--numCounts[nums[i - k]] == 0)
				numCounts.erase(nums[i - k]);
			++numCounts[nums[i]];
			sum += nums[i] - nums[i - k];
			if (numCounts.size() >= m && sum > maximumSum)
				maximumSum = sum;
		}
		return maximumSum;
	}
};
```