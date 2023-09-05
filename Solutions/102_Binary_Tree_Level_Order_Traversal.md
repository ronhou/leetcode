# BFS
```C++
std::vector<std::vector<int>> levelOrder(TreeNode *root)
{
	std::vector<std::vector<int>> levelValues;
	if (root == nullptr)
		return levelValues;

	std::vector<TreeNode *> nodes;
	std::queue<TreeNode *> nodeQueue;
	nodeQueue.push(root);
	while (!nodeQueue.empty())
	{
		while (!nodeQueue.empty())
		{
			nodes.push_back(nodeQueue.front());
			nodeQueue.pop();
		}

		std::vector<int> values;
		for (TreeNode *node : nodes)
		{
			values.push_back(node->val);
			if (node->left)
				nodeQueue.push(node->left);
			if (node->right)
				nodeQueue.push(node->right);
		}
		nodes.clear();
		levelValues.emplace_back(values);
	}
	return levelValues;
}
```