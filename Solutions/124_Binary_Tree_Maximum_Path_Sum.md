# 递归
 * 递归计算以root为端结点的最大路径和maxPathSum（root必须是path路径的端点）
 * 递归过程中顺便更新题目要求的最大路径和maxSum
 * `maxSum = max(maxSum, maxPathSum, maxLeftPathSum + maxRightPathSum + root->val)`

```C++
/**
 * 递归计算以root为端结点的最大路径和maxPathSum（root必须是path路径的端点）
 * 递归过程中顺便更新题目要求的最大路径和maxSum
 * maxSum = max(maxSum, maxPathSum, maxLeftPathSum + maxRightPathSum + root->val)
 */
int maximumPathSum(TreeNode *root, int &maxSum)
{
	// 测试用例允许单个结点
	// maxRootSum保存以root为端结点的路径的最大和（root必须是path路径的端点）
	int maxPathSum = root->val;
	maxSum = std::max(maxSum, maxPathSum);

	int maxLeftPathSum = 0;
	if (root->left != nullptr)
	{
		maxLeftPathSum = maximumPathSum(root->left, maxSum);
		maxPathSum = std::max(maxPathSum, root->val + maxLeftPathSum);
	}

	int maxRightPathSum = 0;
	if (root->right != nullptr)
	{
		maxRightPathSum = maximumPathSum(root->right, maxSum);
		maxPathSum = std::max(maxPathSum, root->val + maxRightPathSum);
	}
	maxSum = std::max(maxSum, maxPathSum);

	if (root->left != nullptr && root->right != nullptr)
	{
		// 最后再以root为中间结点更新一次maxSum
		maxSum = std::max(maxSum, maxLeftPathSum + maxRightPathSum + root->val);
	}

	return maxPathSum;
}

class Solution
{
public:
	int maxPathSum(TreeNode *root)
	{
		int maxSum = INT_MIN;
		maximumPathSum(root, maxSum);
		return maxSum;
	}
};
```