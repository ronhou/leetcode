# 中序遍历BST
中序遍历BST，遍历过程中更新其索引（使用引用参数），当索引等于k时立即返回该结点
```C++
TreeNode *kthNodeOfBST(TreeNode *root, int &index, int k)
{
	if (root->left != nullptr)
	{
		TreeNode *node = kthNodeOfBST(root->left, index, k);
		if (node != nullptr)
			return node;
	}
	if (++index == k)
		return root;
	if (root->right != nullptr)
	{
		TreeNode *node = kthNodeOfBST(root->right, index, k);
		if (node != nullptr)
			return node;
	}
	return nullptr;
}
```