# 广度优先搜索（BFS）
```C++
void populateNextToRight(Node *root)
{
	std::queue<Node *> nodeQueue;
	if (root != NULL)
		nodeQueue.push(root);
	while (!nodeQueue.empty())
	{
		// 收集这一层的结点
		std::vector<Node *> nodes;
		while (!nodeQueue.empty())
		{
			Node *node = nodeQueue.front();
			nodeQueue.pop();
			if (node->left != NULL)
				nodes.push_back(node->left);
			if (node->right != NULL)
				nodes.push_back(node->right);
		}
		// 链接这一层的结点
		for (int i = 0; i < int(nodes.size()) - 1; i++)
			nodes[i]->next = nodes[i + 1];
		// 压入队列
		for (Node *node : nodes)
			nodeQueue.push(node);
	}
}
```