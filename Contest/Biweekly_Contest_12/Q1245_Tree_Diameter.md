1. 将edges边数组转换成一棵树结构，保证O(N)的空间复杂度就行，O(N^2)太大了，而且没必要；
2. 默认0是整棵树的根，根的深度等于所有子树的深度的最大值加1；
3. 计算所有节点的子树的最大深度，顺便计算以该节点为子树的直径，所有直径取最大值即为该题的答案。
```
class Solution {
public:
	int treeDiameter(vector<vector<int>>& edges) {
		vector<vector<int>> tree;
		transformEdgesToTree(edges, tree);

		vector<bool> flags(tree.size(), false);
		size_t nMaxDiameter = 0;
		calcTreeDepth(tree, flags, 0, nMaxDiameter);
		return nMaxDiameter;
	}

	// 将传入的边转换成一个我们可以理解的树
	void transformEdgesToTree(
		const vector<vector<int>>& edges,
		vector<vector<int>>& tree)
	{
		tree.clear();
		tree.resize(edges.size() + 1);

		for (size_t i = 0; i < edges.size(); ++i) {
			tree[edges[i][0]].push_back(edges[i][1]);
			tree[edges[i][1]].push_back(edges[i][0]);
		}
	}

	size_t calcTreeDepth(
		const vector<vector<int>>& tree,
		vector<bool> flags,
		size_t nRootNode,
		size_t& nMaxDiameter)
	{
		flags[nRootNode] = true;

		size_t nMaxDepth = 0, nSecondDepth = 0;
		for (size_t i = 0; i < tree[nRootNode].size(); ++i)
		{
			int nNode = tree[nRootNode][i];
			if (flags[nNode])
				continue;

			size_t nDepth = calcTreeDepth(tree, flags, nNode, nMaxDiameter);
			if (nDepth > nMaxDepth) {
				nSecondDepth = nMaxDepth;
				nMaxDepth = nDepth;
			}
			else if (nDepth > nSecondDepth)
			{
				nSecondDepth = nDepth;
			}
		}

		nMaxDiameter = max(nMaxDiameter, nMaxDepth + nSecondDepth);
		return nMaxDepth + 1;
	}
};
```