# 动态规划
## 最大连续子序列和 + 动态规划
+ 最大连续子序列和可参考 [53. Maximum Subarray](./Solutions/53_Maximum_Subarray.md)
+ 分别计算最大前缀和：`maxPrefixSum[i] = max(maxPrefixSum[i-1], prefixSum)` 和 最大后缀和： `maxSuffixSum[i] = max(maxSuffixSum[i+1], suffixSum)`，取`maxPrefixSum[k]+maxSuffixSum[k+1]`最大值
+ 取两个最大值的较大值

```C++
class Solution
{
public:
	int maxSubarraySumCircular(std::vector<int> &nums)
	{
		int maxSum = INT_MIN, sum = 0;
		size_t beginIndex = 0;
		for (size_t i = 0; i < nums.size(); i++)
		{
			if (sum <= 0)
			{
				sum = nums[i];
				beginIndex = i;
			}
			else
			{
				sum += nums[i];
			}
			maxSum = std::max(maxSum, sum);
		}

		std::vector<int> maxPrefixSum(nums.size(), nums.front());
		sum = nums.front();
		for (size_t i = 1; i < nums.size(); i++)
		{
			sum += nums[i];
			maxPrefixSum[i] = std::max(maxPrefixSum[i - 1], sum);
		}

		std::vector<int> maxSuffixSum(nums.size(), nums.back());
		sum = nums.back();
		for (int i = int(nums.size()) - 2; i >= 0; i--)
		{
			sum += nums[i];
			maxSuffixSum[i] = std::max(maxSuffixSum[i + 1], sum);
		}

		for (int i = 0; i < int(nums.size()) - 2; i++)
			maxSum = std::max(maxSum, maxPrefixSum[i] + maxSuffixSum[i + 1]);
		return maxSum;
	}
};
```

## 最大连续子序列和 + 最小连续子序列和
分别计算最大连续子序列和，最小连续子序列和，总和。取`max(最大连续子序列和, 总和 - 最小连续子序列和)`
```C++
class Solution
{
public:
	int maxSubarraySumCircular(std::vector<int> &nums)
	{
		int maxSum = INT_MIN, dpMaxSum = 0;
		int minSum = INT_MAX, dpMinSum = 0;
		int sum = 0;
		for (auto &&num : nums)
		{
			dpMaxSum = std::max(dpMaxSum + num, num);
			maxSum = std::max(maxSum, dpMaxSum);
			dpMinSum = std::min(dpMinSum + num, num);
			minSum = std::min(minSum, dpMinSum);
			sum += num;
		}
		return maxSum < 0 ? maxSum : std::max(maxSum, sum - minSum);
	}
};
```