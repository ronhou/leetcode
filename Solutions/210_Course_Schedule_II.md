# 拓扑排序
```C++
std::vector<int> getTopologicalOrdering(std::vector<int> &indegrees, std::vector<std::vector<int>> &graph)
{
	std::vector<int> order;
	std::queue<int> q;
	for (int i = 0; i < indegrees.size(); i++)
	{
		if (indegrees[i] <= 0)
			q.push(i);
	}
	while (!q.empty())
	{
		int curr = q.front();
		q.pop();
		order.push_back(curr);
		for (auto &&next : graph[curr])
		{
			if (--indegrees[next] == 0)
				q.push(next);
		}
	}

	if (order.size() != graph.size())
		order.clear();
	return order;
}

std::vector<int> findOrder(int numCourses, std::vector<std::vector<int>> &prerequisites)
{
	std::vector<int> indegrees(numCourses, 0);
	std::vector<std::vector<int>> graph(numCourses);
	for (auto &&prerequisite : prerequisites)
	{
		++indegrees[prerequisite[0]];
		graph[prerequisite[1]].push_back(prerequisite[0]);
	}
	return getTopologicalOrdering(indegrees, graph);
}
```