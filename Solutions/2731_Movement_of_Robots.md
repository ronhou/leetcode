> [2731. Movement of Robots](https://leetcode.cn/problems/movement-of-robots/)

## 脑筋急转弯 + 排序 + 计算
> [移动机器人 - 力扣官方题解：脑筋急转弯 + 排序](https://leetcode.cn/problems/movement-of-robots/solutions/2470646/yi-dong-ji-qi-ren-by-leetcode-solution-tm4n/)
```
class Solution
{
public:
	int sumDistance(std::vector<int> &nums, std::string s, int d)
	{
		size_t count = nums.size();
		std::vector<long long> positions(count, 0);
		for (size_t i = 0; i < count; i++)
			positions[i] = (long long)nums[i] + d * (s[i] == 'R' ? 1 : -1);
		std::sort(positions.begin(), positions.end());

		const long long Mod = 1E9 + 7;
		long long sumDis = 0;
		for (size_t i = 1; i < positions.size(); i++)
			sumDis = (sumDis + i * (count - i) * (positions[i] - positions[i - 1]) % Mod) % Mod;
		return sumDis;
	}
};
```