# BFS + 哈希表
1. 使用BFS按层次遍历的方式获取最后一层（最深）的叶子结点
2. 使用哈希表保存各结点的父节点
3. 逆向遍历各叶子结点的父节点，相交位置即为最近公共祖先
> 这一题同#[865. Smallest Subtree with all the Deepest Nodes](https://leetcode.cn/problems/smallest-subtree-with-all-the-deepest-nodes/)
```C++
std::vector<TreeNode *> bfsDeepestLeaves(TreeNode *root)
{
	std::vector<TreeNode *> deepestLeaves;
	deepestLeaves.push_back(root);
	std::queue<TreeNode *> nodeQueue;
	nodeQueue.push(root);
	std::vector<TreeNode *> nextDepthNodes;
	while (!nodeQueue.empty())
	{
		while (!nodeQueue.empty())
		{
			TreeNode *node = nodeQueue.front();
			nodeQueue.pop();
			if (node->left)
				nextDepthNodes.push_back(node->left);
			if (node->right)
				nextDepthNodes.push_back(node->right);
		}
		if (nextDepthNodes.size() > 0)
		{
			// 更新最深的叶子结点；更新BFS队列
			deepestLeaves.clear();
			for (TreeNode *node : nextDepthNodes)
			{
				deepestLeaves.push_back(node);
				nodeQueue.push(node);
			}
			nextDepthNodes.clear();
		}
	}
	return deepestLeaves;
}

void getNodesParent(TreeNode *root, std::unordered_map<TreeNode *, TreeNode *> &nodeParentMap)
{
	if (root->left)
	{
		nodeParentMap[root->left] = root;
		getNodesParent(root->left, nodeParentMap);
	}
	if (root->right)
	{
		nodeParentMap[root->right] = root;
		getNodesParent(root->right, nodeParentMap);
	}
}

TreeNode *lcaDeepestLeaves(TreeNode *root)
{
	std::vector<TreeNode *> deepestLeaves = bfsDeepestLeaves(root);
	if (deepestLeaves.size() == 1)
		return deepestLeaves[0];

	std::unordered_map<TreeNode *, TreeNode *> nodeParentMap;
	getNodesParent(root, nodeParentMap);
	std::unordered_map<TreeNode *, int> ancestors;
	TreeNode *node = deepestLeaves[0];
	int index = 0;
	while (node != nullptr)
	{
		ancestors[node] = index++;
		node = nodeParentMap[node];
	}

	TreeNode *lcaNode;
	int lcaIndex = -1;
	for (int i = 1; i < deepestLeaves.size(); i++)
	{
		node = deepestLeaves[i];
		while (node != nullptr && ancestors.count(node) <= 0)
			node = nodeParentMap[node];
		if (node != nullptr && ancestors[node] > lcaIndex)
		{
			lcaNode = node;
			lcaIndex = ancestors[node];
		}
	}
	return lcaNode;
}
```