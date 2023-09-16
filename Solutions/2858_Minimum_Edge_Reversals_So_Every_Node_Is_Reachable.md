# DFS + DFS/BFS
1. 首先使用`edges`构建一张记录边方向的有向图`graph`，且同时添加反方向的边。
   + 由于此题结点个数较多，且相比之下边数较少，是一个标准的“稀疏图”，因为采用邻接表来保存该图。
2. 从任意结点出发，比如结点0，使用DFS的方式，计算构成以0为根结点的树需要反转多少条边。
   + 构建以`rootIdx`为根的子树所需要反转的边的数量 等于 构建各个子树反转的边树的和 加上 根是否有指向这个孩子的边。
   + 即：`reversals[root] = sum(reversals[neighbor(child)] + 1/0)`
3. 最后，采取DFS或BFS的方式，从根节点0出发，更新所有构建以其它结点为根的树所需反转边的数量。
   + 如果边是由根（rootIndex）指向相邻孩子（neighbor child），那么构建以该孩子为根的树所需反转的边的数量 等于 父结点（即根）的次数 加 1。
     + `ans[child] = ans[root] + 1`
   + 否则，等于父节点所需次数减一：`ans[child] = ans[root] - 1`

```C++ [DFS + DFS/BFS]
class Solution
{
public:
	std::vector<int> minEdgeReversals(int n, std::vector<std::vector<int>> &edges)
	{
		std::vector<std::vector<std::pair<int, bool>>> graph(n);
		for (auto &&edge : edges)
		{
			graph[edge[0]].push_back({edge[1], true});
			graph[edge[1]].push_back({edge[0], false});
		}

		std::vector<int> ans(n, 0);
		ans[0] = dfsCountEdgeReversals(graph, -1, 0);
		// bfs也可以更新
		dfsUpdateEdgeReversals(graph, ans, -1, 0);
		return ans;
	}

	int dfsCountEdgeReversals(std::vector<std::vector<std::pair<int, bool>>> &graph, int parentIdx, int rootIdx)
	{
		int count = 0;
		for (auto &&nodeInfo : graph[rootIdx])
		{
			if (nodeInfo.first != parentIdx)
			{
				int subCount = dfsCountEdgeReversals(graph, rootIdx, nodeInfo.first);
				count += subCount + (nodeInfo.second ? 0 : 1);
			}
		}
		return count;
	}

	// bfs也可以做到
	void dfsUpdateEdgeReversals(std::vector<std::vector<std::pair<int, bool>>> &graph, std::vector<int> &ans, int parentIdx, int rootIdx)
	{
		for (auto &&nodeInfo : graph[rootIdx])
		{
			if (nodeInfo.first != parentIdx)
			{
				ans[nodeInfo.first] = ans[rootIdx] + (nodeInfo.second ? 1 : -1);
				dfsUpdateEdgeReversals(graph, ans, rootIdx, nodeInfo.first);
			}
		}
	}
};
```