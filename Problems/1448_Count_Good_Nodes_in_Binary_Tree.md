# 递归
``` C++
int countGoodNodes(TreeNode *root, int maxVal)
{
	if (root == nullptr)
	{
		return 0;
	}

	if (root->val >= maxVal)
	{
		return countGoodNodes(root->left, root->val) + countGoodNodes(root->right, root->val) + 1;
	}
	else
	{
		return countGoodNodes(root->left, maxVal) + countGoodNodes(root->right, maxVal);
	}
}
```