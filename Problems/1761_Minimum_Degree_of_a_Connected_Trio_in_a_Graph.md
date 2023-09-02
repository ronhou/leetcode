# 暴力枚举每一个三元组
三元组的度数 = 三个结点的度数和 - 6
```C++ [按邻接矩阵枚举三元组结点]
int minTrioNodeDegree(int n, std::vector<std::vector<int>> &edges)
{
	// 构建无向图的邻接矩阵
	std::vector<std::vector<bool>> connected(n, std::vector<bool>(n, false));
	// 计算每个结点的度数
	std::vector<int> degrees(n, 0);
	for (const std::vector<int> &edge : edges)
	{
		connected[edge[0] - 1][edge[1] - 1] = connected[edge[1] - 1][edge[0] - 1] = true;
		++degrees[edge[0] - 1];
		++degrees[edge[1] - 1];
	}

	// 暴力枚举三个结点构成的三元组
	int minDegree = edges.size();
	for (int i = 0; i < n - 2; i++)
	{
		for (int j = i + 1; j < n - 1; j++)
		{
			if (!connected[i][j])
				continue;
			for (int k = j + 1; k < n; k++)
			{
				if (!connected[i][k] || !connected[j][k])
					continue;
				// 三元组的度数 = 三个结点的度数和 - 6
				minDegree = std::min(minDegree, degrees[i] + degrees[j] + degrees[k] - 6);
			}
		}
	}
	return (minDegree != edges.size()) ? minDegree : -1;
}
```
```C++ [按邻接表枚举三元组结点]
int minTrioNodeDegree(int n, std::vector<std::vector<int>> &edges)
{
	// 构建无向图的邻接表（完整表），用来判断结点是否相连
	std::vector<std::unordered_set<int>> graph(n, std::unordered_set<int>());
	// 构建无向图的邻接表（仅保存序号大于自己的结点），枚举时用
	std::vector<std::vector<int>> greaterGraph(n, std::vector<int>());
	// 计算每个结点的度数
	std::vector<int> degrees(n, 0);
	for (const std::vector<int> &edge : edges)
	{
		int x = edge[0] - 1, y = edge[1] - 1;
		if (x < y)
			greaterGraph[x].push_back(y);
		else
			greaterGraph[y].push_back(x);
		graph[x].insert(y);
		graph[y].insert(x);
		++degrees[x];
		++degrees[y];
	}

	int minDegree = edges.size();
	for (int i = 0; i < n - 2; i++)
	{
		for (int j : greaterGraph[i])
		{
			for (int k : greaterGraph[j])
			{
				if (graph[i].count(k))
				{
					minDegree = std::min(minDegree, degrees[i] + degrees[j] + degrees[k] - 6);
				}
			}
		}
	}
	return (minDegree != edges.size()) ? minDegree : -1;
}
```