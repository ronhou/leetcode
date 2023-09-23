> [35. Search Insert Position](https://leetcode.cn/problems/search-insert-position/)
## 二分查找
```C++ [std]
class Solution
{
public:
	int searchInsert(std::vector<int> &nums, int target)
	{
		return std::distance(nums.begin(), std::lower_bound(nums.begin(), nums.end(), target));
	}
};
```
```C++ [lower_bound]
class Solution
{
public:
	int searchInsert(std::vector<int> &nums, int target)
	{
		int l = 0, r = nums.size(), mid = 0;
		while (l < r)
		{
			mid = (l + r) / 2;
			if (nums[mid] < target)
				l = mid + 1;
			else
				r = mid;
		}
		return l;
	}
};
```
```C++ [std::lower_bound]
class Solution
{
public:
	int searchInsert(std::vector<int> &nums, int target)
	{
		std::vector<int>::iterator first = nums.begin(), last = nums.end(), it;
		std::vector<int>::difference_type count = std::distance(first, last), step = 0;
		while (count > 0)
		{
			it = first;
			step = count / 2;
			std::advance(it, step);
			if (*it < target)
			{
				first = ++it;
				count -= step + 1;
			}
			else
			{
				count = step;
			}
		}
		return std::distance(nums.begin(), first);
	}
};
```