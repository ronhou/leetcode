# 分治
将升序数组分成三部分，将中间结点作为根节点，将左边部分转换为左子树，将右边部分转换为右子树。
左右子树递归处理。

```C++ [递归分治]
class Solution
{
public:
	TreeNode *sortedArrayToBST(std::vector<int> &nums)
	{
		return covertSortedArrayToBST(nums, 0, nums.size() - 1);
	}

private:
	TreeNode *covertSortedArrayToBST(const std::vector<int> &nums, int l, int r)
	{
		int mid = (l + r) / 2;
		TreeNode *left = nullptr;
		if (l < mid)
			left = covertSortedArrayToBST(nums, l, mid - 1);
		TreeNode *right = nullptr;
		if (mid < r)
			right = covertSortedArrayToBST(nums, mid + 1, r);
		return new TreeNode(nums[mid], left, right);
	}
};
```