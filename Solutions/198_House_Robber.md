# 动态规划
每个房子都面临两个选择：不偷或者被偷。依次对房子做出选择：
> 用`robs[i][0]`表示不偷第i间房子能偷到的最大金额，用`robs[i][1]`表示偷第i间房子能偷到的最大金额。
+ 如果不偷当前这间房子，那么目前能偷到的最大现金数（`robs[i][0]`）等于偷上一间房子`（robs[i - 1][1]`）或者不偷上一间房子（`robs[i - 1][0]`）的最大现金数的较大值
  + `robs[i][0] = max(robs[i - 1][0], robs[i - 1][1])`
+ 如果偷当前这间房子，那么上一间房一定不能偷，因此便有：
  + `robs[i][1] = robs[i - 1][0] + nums[i]`

## 空间优化
由上面的分析可知，当前的状态（偷得的最大金额）仅与上一状态有关，我们没有必要保存上一状态之前的状态。这也是动态规划常见的空间优化策略：滚动数组。

我们使用`lastMaxRob0`和`lastMaxRob1`来表示*不偷*和*偷*上一间房所得的最大金额，使用`curMaxRob0`和`curMaxRob1`表示*不偷*和*偷*当前这一间房所得的最大金额，那么便有：
+ `curMaxRob1 = lastMaxRob0 + nums[i]`
+ `curMaxRob0 = max(lastMaxRob0, lastMaxRob1)`

然后滚动更新`lastMaxRob0`和`lastMaxRob1`即可。

```C++ [普通版动态规划]
class Solution
{
public:
	int rob(std::vector<int> &nums)
	{
		// robs[i].first表示偷第i间房，robs[i].second表示不偷第i间房
		std::vector<std::pair<int, int>> robs(nums.size(), {0, 0});
		robs[0].first = nums[0];
		for (int i = 1; i < nums.size(); i++)
		{
			robs[i].first = robs[i - 1].second + nums[i];
			robs[i].second = std::max(robs[i - 1].first, robs[i - 1].second);
		}
		return std::max(robs[nums.size() - 1].first, robs[nums.size() - 1].second);
	}
};
```
```C++ [空间优化]
class Solution
{
public:
	int rob(std::vector<int> &nums)
	{
		int lastMaxRob = nums[0], curMaxRob = nums[0], maxRob0 = 0;
		for (int i = 1; i < nums.size(); i++)
		{
			curMaxRob = maxRob0 + nums[i];
			maxRob0 = std::max(maxRob0, lastMaxRob);
			lastMaxRob = curMaxRob;
		}
		return std::max(curMaxRob, maxRob0);
	}
};
```