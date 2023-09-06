# 中序遍历
中序遍历二叉树，遍历过程中更新“前一个”结点（使用引用参数）。处理当前结点时，比较前一个结点和当前结点的值，如果验证错误，立即返回false。
```C++
bool inorderCheckIsBST(TreeNode *root, TreeNode *&prevNode)
{
	if (root->left != nullptr)
	{
		if (!inorderCheckIsBST(root->left, prevNode))
			return false;
	}
	if (prevNode != nullptr && prevNode->val >= root->val)
		return false;
	prevNode = root;
	if (root->right != nullptr)
	{
		if (!inorderCheckIsBST(root->right, prevNode))
			return false;
	}
	return true;
}
```