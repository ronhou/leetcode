# 哈希/排序
```C++ [排序]
int longestConsecutiveBySort(std::vector<int> &nums)
{
	std::sort(nums.begin(), nums.end());
	int longest = 0;
	for (int l = 0, r = 0; r < nums.size(); r++)
	{
		if (r > 0 && nums[r] > nums[r - 1] + 1)
		{
			l = r;
		}
		longest = std::max(longest, nums[r] - nums[l] + 1);
	}
	return longest;
}
```
```C++ [哈希]
int longestConsecutiveByHash(std::vector<int> &nums)
{
	std::unordered_set<int> numSet;
	for (const int& num : nums)
	{
		numSet.insert(num);
	}

	int longest = 0;
	for (const int& num : nums)
	{
		if (numSet.count(num - 1))
		{
			continue;
		}
		int nextNum = num + 1;
		while (numSet.count(nextNum))
		{
			++nextNum;
		}
		longest = std::max(longest, nextNum - num);
	}
	return longest;
}
```