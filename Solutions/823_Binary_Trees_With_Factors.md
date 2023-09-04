# 动态规划+哈希表
```C++
int calcFactoredBinaryTreesCount(std::vector<int> &nums)
{
	const int Mod = 1e9 + 7;
	std::sort(nums.begin(), nums.end());
	std::unordered_map<int, unsigned long long> counts;
	int totalCount = 0;
	for (int i = 0; i < nums.size(); i++)
	{
		counts[nums[i]] = 1;
		for (int j = 0; j < i; j++)
		{
			if (nums[i] % nums[j] == 0 && counts[nums[i] / nums[j]] > 0)
			{
				counts[nums[i]] = (counts[nums[i]] + counts[nums[j]] * counts[nums[i] / nums[j]] % Mod) % Mod;
			}
		}
		totalCount = (totalCount + counts[nums[i]]) % Mod;
	}
	return totalCount;
}
```