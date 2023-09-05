# BFS
```C++
std::vector<double> averageOfLevels(TreeNode *root)
{
	std::vector<double> averages;
	std::vector<TreeNode *> nodes;
	std::queue<TreeNode *> nodeQueue;
	nodeQueue.push(root);

	double sum = 0;
	while (!nodeQueue.empty())
	{
		while (!nodeQueue.empty())
		{
			nodes.push_back(nodeQueue.front());
			nodeQueue.pop();
		}

		sum = 0;
		for (TreeNode *node : nodes)
		{
			sum += node->val;
			if (node->left)
				nodeQueue.push(node->left);
			if (node->right)
				nodeQueue.push(node->right);
		}
		averages.push_back(sum / nodes.size());
		nodes.clear();
	}
	return averages;
}
```