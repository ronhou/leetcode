# 递归存储路径
递归存储根到结点的路径，对比两条路径

```C++
bool getPathToRoot(TreeNode *root, TreeNode *node, std::vector<TreeNode *> &path)
{
	path.push_back(root);
	if (root == node)
		return true;

	if (root->left && getPathToRoot(root->left, node, path))
		return true;
	if (root->right && getPathToRoot(root->right, node, path))
		return true;
	path.pop_back();
	return false;
}

TreeNode *closestCommonAncestor(TreeNode *root, TreeNode *p, TreeNode *q)
{
	std::vector<TreeNode *> pathP, pathQ;
	getPathToRoot(root, p, pathP);
	getPathToRoot(root, q, pathQ);

	int index = 0;
	while (index < pathP.size() && index < pathQ.size() && pathP[index] == pathQ[index])
		++index;
	return pathP[--index];
}
```