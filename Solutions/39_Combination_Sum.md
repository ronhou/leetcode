# 递归回溯

```C++
void combineSum(const std::vector<int> &candidates, int target, std::vector<std::vector<int>>& combs, std::vector<int>& comb, int sum, int index)
{
	if (sum == target)
		combs.push_back(comb);
	if (index >= candidates.size() || sum >= target || candidates[index] > target - sum)
		return;

	int sz = comb.size();
	combineSum(candidates, target, combs, comb, sum, index + 1);
	for (int i = 0; i < target / candidates[index]; i++)
	{
		comb.push_back(candidates[index]);
		sum += candidates[index];
		combineSum(candidates, target, combs, comb, sum, index + 1);
	}
	comb.resize(sz);
}

class Solution
{
public:
	std::vector<std::vector<int>> combinationSum(std::vector<int> &candidates, int target)
	{
		std::vector<std::vector<int>> combs;
		std::vector<int> comb;
		std::sort(candidates.begin(), candidates.end());
		combineSum(candidates, target, combs, comb, 0, 0);
		return combs;
	}
};
```