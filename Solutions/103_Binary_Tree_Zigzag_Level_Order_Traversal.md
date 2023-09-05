# BFS
```C++
std::vector<std::vector<int>> zigzagLevelOrder(TreeNode *root)
{
	std::vector<std::vector<int>> levelValues;
	if (root == nullptr)
		return levelValues;

	int level = 0;
	std::vector<TreeNode *> nodes;
	std::vector<int> values;
	std::queue<TreeNode *> nodeQueue;
	nodeQueue.push(root);
	while (!nodeQueue.empty())
	{
		while (!nodeQueue.empty())
		{
			nodes.push_back(nodeQueue.front());
			nodeQueue.pop();
		}

		for (TreeNode *node : nodes)
		{
			values.push_back(node->val);
			if (node->left)
				nodeQueue.push(node->left);
			if (node->right)
				nodeQueue.push(node->right);
		}
		if (level % 2 > 0)
			std::reverse(values.begin(), values.end());
		levelValues.push_back(values);
		values.clear();
		nodes.clear();
		++level;
	}
	return levelValues;
}
```