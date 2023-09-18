## Kadane算法
### 最大连续子序列和
#### 动态规划
dp[i]表示以i结尾的最大连续子序列和，状态转移方程是：`dp[i] = max(dp[i-1] + nums[i], num)`。
那么，整个数组的最大连续子序列和是：`maxSum = max(maxSum, dp[i])`
由于状态转移过程中只使用了上一状态，因此上面的状态转移方程还可以进行空间优化：`dpSum = max(dpSum + num, num)`。

```C++
class Solution
{
public:
	int maxSubArray(std::vector<int> &nums)
	{
		int maxSum = INT_MIN, sum = 0;
		for (auto &&num : nums)
		{
			sum = std::max(sum + num, num);
			maxSum = std::max(maxSum, sum);
		}
		return maxSum;
	}
};
```
