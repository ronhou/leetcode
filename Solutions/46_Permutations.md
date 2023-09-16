# 递归回溯

```C++
class Solution
{
public:
	std::vector<std::vector<int>> permute(std::vector<int> &nums)
	{
		std::vector<std::vector<int>> permutations;
		std::vector<bool> used(nums.size(), false);
		std::vector<int> permutation;
		permute(nums, permutations, used, permutation);
		return permutations;
	}

private:
	void permute(const std::vector<int> &nums, std::vector<std::vector<int>> &permutations, std::vector<bool> &used, std::vector<int> &permutation)
	{
		if (permutation.size() >= nums.size())
		{
			permutations.push_back(permutation);
			return;
		}
		for (int i = 0; i < nums.size(); i++)
		{
			if (!used[i])
			{
				used[i] = true;
				permutation.push_back(nums[i]);
				permute(nums, permutations, used, permutation);
				permutation.pop_back();
				user[i] = false;
			}
		}
	}
};
```