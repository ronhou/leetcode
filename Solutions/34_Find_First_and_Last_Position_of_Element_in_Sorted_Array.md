> [34. Find First and Last Position of Element in Sorted Array](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

# 二分查找
先使用`std::lower_bound`查找首个元素，然后再使用`std::upper_bound`查找最后一个元素

```C++
class Solution
{
public:
	std::vector<int> searchRange(std::vector<int> &nums, int target)
	{
		if (nums.empty())
			return {-1, -1};

		auto itBegin = nums.begin(), itEnd = nums.end();
		std::vector<int>::iterator itFirst = std::lower_bound(itBegin, itEnd, target);
		if (itFirst == itEnd || *itFirst != target)
			return {-1, -1};

		std::vector<int>::iterator itLast = std::upper_bound(itBegin, itEnd, target);
		--itLast;
		return {
			int(std::distance(itBegin, itFirst)),
			int(std::distance(itBegin, itLast)),
		};
	}
};
```