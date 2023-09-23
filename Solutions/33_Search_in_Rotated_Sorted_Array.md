> [33. Search in Rotated Sorted Array](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

# 二分查找
原本有序的数组被分成了左右两部分，左边部分都是大于等于`nums[0]`，右边部分都小于`nums[0]`。
分情况进行二分查找：
+ 如果中间数字`nums[mid]`等于`target`，那么立即返回`mid`
+ 如果中间数字`nums[mid]`小于`target`
  + 且这两个数字位于数组的同一边（都在左边部分，或都在右边部分），那么更新左边界
  + 否则更新右边界
+ 反之，如果中间数字`nums[mid]`大于`target`
  + 且这两个数字位于数组的同一边（都在左边部分，或都在右边部分），那么更新右边界
  + 否则更新左边界

```C++
class Solution
{
public:
	int search(std::vector<int> &nums, int target)
	{
		int l = 0, r = nums.size() - 1;
		while (l <= r)
		{
			int mid = (l + r) / 2;
			if (nums[mid] == target)
				return mid;
			if (nums[mid] < target)
			{
				if (target < nums[0] || nums[0] <= nums[mid])
					l = mid + 1;
				else
					r = mid - 1;
			}
			else
			{
				if (nums[mid] < nums[0] || nums[0] <= target)
					r = mid - 1;
				else
					l = mid + 1;
			}
		}
		return -1;
	}
};
```