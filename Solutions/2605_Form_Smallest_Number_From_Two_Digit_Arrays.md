### 哈希表判重，排序取最小值
+ 哈希表判重，如果两个数组中出现了重复的数字，则直接返回该数字
+ 分别对两个数字按从小到大进行排序，分别取最小值，组合成最小数字即可

```C++
int formMinNumber(std::vector<int> &nums1, std::vector<int> &nums2)
{
	std::sort(nums2.begin(), nums2.end());
	std::unordered_set<int> numSet1(nums1.begin(), nums1.end());
	for (int num : nums2)
	{
		if (numSet1.count(num) > 0)
			return num;
	}

	std::sort(nums1.begin(), nums1.end());
	int minNum = std::min(nums1[0], nums2[0]);
	int maxNum = std::max(nums1[0], nums2[0]);
	return minNum * 10 + maxNum;
}
```