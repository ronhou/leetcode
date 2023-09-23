> [162. Find Peak Element](https://leetcode.cn/problems/find-peak-element/)

## 模拟
题设中假定：`nums[-1] = nums[n] = -∞`，那么找到第一个下降的情况即可。
```C++
class Solution
{
public:
	int findPeakElement(std::vector<int> &nums)
	{
		for (size_t i = 1; i < nums.size(); i++)
		{
			if (nums[i] < nums[i - 1])
				return i - 1;
		}
		return nums.size() - 1;
	}
};
```

## 二分
+ 如果`nums[k-1] < nums[k] > nums[k+1]`，那么k位置其中一个峰值
+ 如果`nums[k-1] > nums[k] < nums[k+1]`，那么k位置是谷底，左右都可能有峰值
+ 如果`nums[k-1] < nums[k] < nums[k+1]`，那么峰值在k的右边
+ 如果`nums[k-1] > nums[k] > nums[k+1]`，那么峰值在k的左边
```C++
class Solution
{
public:
	int findPeakElement(std::vector<int> &nums)
	{
		int l = 0, r = nums.size() - 1;
		while (l < r)
		{
			int mid = (l + r) / 2;
			if ((mid == nums.size() - 1 || nums[mid] > nums[mid + 1]))
			{
				if (mid == 0 || nums[mid - 1] < nums[mid])
					return mid;
				else
					r = mid - 1;
			}
			else
			{
				l = mid + 1;
			}
		}
		return l;
	}
};
```