# 中序遍历BST
1. 使用辅助数组，作为中序遍历的引用形参
   1. 中序遍历BST即可得到升序数组
   2. 求任意两个元素之差，取最小差即可
2. 使用引用形参，中序遍历过程更新“前一个”和“最小差”

```C++ [辅助数组]
void inorderTraversal(TreeNode *root, std::vector<TreeNode *> &nodes)
{
	if (root->left != nullptr)
		inorderTraversal(root->left, nodes);
	nodes.push_back(root);
	if (root->right != nullptr)
		inorderTraversal(root->right, nodes);
}

int getMinDiffOfBST(TreeNode *root)
{
	std::vector<TreeNode *> nodes;
	inorderTraversal(root, nodes);

	int minDiff = INT_MAX;
	for (int i = 1; i < nodes.size(); i++)
		minDiff = std::min(minDiff, nodes[i]->val - nodes[i - 1]->val);
	return minDiff;
}
```
```C++ [前一个结点]
oid inorderTraversal(TreeNode *root, TreeNode *&prevNode, int &minDiff)
{
	if (root->left != nullptr)
		inorderTraversal(root->left, prevNode, minDiff);
	if (prevNode != nullptr)
		minDiff = std::min(minDiff, root->val - prevNode->val);
	prevNode = root;
	if (root->right != nullptr)
		inorderTraversal(root->right, prevNode, minDiff);
}

int getMinDiffOfBST(TreeNode *root)
{
	TreeNode *prevNode = nullptr;
	int minDiff = INT_MAX;
	inorderTraversal(root, prevNode, minDiff);
	return minDiff;
}
```