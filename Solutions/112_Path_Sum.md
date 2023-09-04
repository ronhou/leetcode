# 递归
```C++
bool hasPathSum(TreeNode *root, int targetSum)
{
	if (root == nullptr)
		return false;

	targetSum -= root->val;
	if (root->left == nullptr && root->right == nullptr)
		return targetSum == 0;

	return root->left != nullptr && hasPathSum(root->left, targetSum)
		|| root->right != nullptr && hasPathSum(root->right, targetSum);
}
```