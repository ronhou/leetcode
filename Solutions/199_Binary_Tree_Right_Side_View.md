# BFS
```C++
std::vector<int> rightSideView(TreeNode *root)
{
	std::vector<int> rightNodeValues;
	if (root == nullptr)
		return rightNodeValues;

	std::vector<TreeNode *> nodes;
	std::queue<TreeNode *> nodeQueue;
	nodeQueue.push(root);
	while (!nodeQueue.empty())
	{
		rightNodeValues.push_back(nodeQueue.back()->val);
		while (!nodeQueue.empty())
		{
			nodes.push_back(nodeQueue.front());
			nodeQueue.pop();
		}
		for (TreeNode *node : nodes)
		{
			if (node->left)
				nodeQueue.push(node->left);
			if (node->right)
				nodeQueue.push(node->right);
		}
		nodes.clear();
	}
	return rightNodeValues;
}
```