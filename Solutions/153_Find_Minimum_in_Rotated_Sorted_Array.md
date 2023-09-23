> [153. Find Minimum in Rotated Sorted Array](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

# 二分查找
[33. Search in Rotated Sorted Array](./Solutions/33_Search_in_Rotated_Sorted_Array.md) 的变版。前面那一题是有明确的target需要查找，而这一题改成了查找最小值。类似的思路。
+ 如果`nums[mid] >= nums[0]`，那么最小值在右侧；
+ 反之，最小值在左侧。

```C++
class Solution
{
public:
	int findMin(std::vector<int> &nums)
	{
		if (nums.back() >= nums.front())
			return nums.front();

		int l = 0, r = nums.size() - 1;
		while (l < r)
		{
			int mid = (l + r) / 2;
			if (nums[mid] >= nums[0])
				l = mid + 1;
			else
				r = mid;
		}
		return nums[l];
	}
};
```