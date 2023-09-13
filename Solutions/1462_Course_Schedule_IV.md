# Floyed计算DAG最短路径
```C++ [Floyed]
class Solution
{
public:
	std::vector<bool> checkIfPrerequisite(int numCourses, std::vector<std::vector<int>> &prerequisites, std::vector<std::vector<int>> &queries)
	{
		std::vector<std::vector<int>> graph(numCourses, std::vector<int>(numCourses, 0));
		for (auto &&prerequisite : prerequisites)
			graph[prerequisite[0]][prerequisite[1]] = 1;

		// Floyed求最短路径
		for (int cur = 0; cur < numCourses; cur++)
		{
			for (int prev = 0; prev < numCourses; prev++)
			{
				if (graph[prev][cur] > 0)
				{
					for (int next = 0; next < numCourses; next++)
					{
						if (graph[cur][next] > 0)
						{
							if (graph[prev][next] == 0)
								graph[prev][next] = graph[prev][cur] + graph[cur][next];
							else
								graph[prev][next] = std::min(graph[prev][next], graph[prev][cur] + graph[cur][next]);
						}
					}
				}
			}
		}

		std::vector<bool> ans;
		for (auto &&query : queries)
			ans.push_back(graph[query[0]][query[1]] > 0);
		return ans;
	}
};
```

# BFS/DFS + 拓扑排序
采取BFS或DFS的方式进行拓扑排序，拓扑排序的过程中更新结点之间的依赖关系：基于当前结点cur，如果存在[cur, next]的有向边依赖（next依赖cur），那么，对于cur所有依赖的结点，next也必须依赖。

```C++ [BFS + 拓扑排序]
class Solution
{
public:
	std::vector<bool> checkIfPrerequisite(int numCourses, std::vector<std::vector<int>> &prerequisites, std::vector<std::vector<int>> &queries)
	{
		std::vector<std::vector<int>> graph(numCourses, std::vector<int>(numCourses, 0));
		std::vector<int> indegrees(numCourses, 0);
		for (auto &&prerequisite : prerequisites)
		{
			graph[prerequisite[0]][prerequisite[1]] = 1;
			++indegrees[prerequisite[1]];
		}

		std::queue<int> q;
		for (int i = 0; i < numCourses; i++)
		{
			if (indegrees[i] == 0)
				q.push(i);
		}
		while (!q.empty())
		{
			// a. 拓扑排序：取出入度为0的结点
			int cur = q.front();
			q.pop();
			for (int next = 0; next < numCourses; next++)
			{
				// A. 存在[cur, next]的有向边（依赖）
				if (graph[cur][next] == 1)
				{
					/**
					 * 拓扑排序：
					 * b. （因为从DAG中摘取cur结点的缘故）next的入度减1
					 * c. 如果结点的入度变为0，那么可以将该结点添加到拓扑队列
					 */
					--indegrees[next];
					if (indegrees[next] == 0)
						q.push(next);
					/**
					 * 以cur结点为中间点，更新next结点的依赖：
					 * B. 基于A的前提，如果cur依赖于prev，那么next结点必定依赖于prev
					 */
					for (int prev = 0; prev < numCourses; prev++)
					{
						if (graph[prev][cur] > 0)
						{
							// 这里将其更新成任意大于1的值都行，取min值是因为我想看下能否按照Floyed算法思路计算最短路径
							if (graph[prev][next] == 0)
								graph[prev][next] = graph[prev][cur] + 1;
							else
								graph[prev][next] = std::min(graph[prev][next], graph[prev][cur] + 1);
						}
					}
				}
			}
		}

		std::vector<bool> ans;
		for (auto &&query : queries)
			ans.push_back(graph[query[0]][query[1]] > 0);
		return ans;
	}
};
```
```C++ [DFS + 拓扑排序]
class Solution
{
public:
	std::vector<bool> checkIfPrerequisite(int numCourses, std::vector<std::vector<int>> &prerequisites, std::vector<std::vector<int>> &queries)
	{
		std::vector<std::vector<int>> graph(numCourses, std::vector<int>(numCourses, 0));
		std::vector<int> indegrees(numCourses, 0);
		for (auto &&prerequisite : prerequisites)
		{
			graph[prerequisite[0]][prerequisite[1]] = 1;
			++indegrees[prerequisite[1]];
		}

		std::vector<int> roots;
		for (int cur = 0; cur < indegrees.size(); cur++)
		{
			if (indegrees[cur] == 0)
				roots.push_back(cur);
		}
		for (auto &&cur : roots)
			dfsTopologicOrdering(graph, indegrees, cur);

		std::vector<bool> ans;
		for (auto &&query : queries)
			ans.push_back(graph[query[0]][query[1]] > 0);
		return ans;
	}

private:
	void dfsTopologicOrdering(std::vector<std::vector<int>> &graph, std::vector<int> &indegrees, int cur)
	{
		for (int next = 0; next < graph[cur].size(); next++)
		{
			if (graph[cur][next] == 1)
			{
				// 以cur结点为中间结点，更新prev和next的依赖关系
				for (int prev = 0; prev < graph.size(); prev++)
				{
					if (graph[prev][cur] > 0)
					{
						// 这里将其更新成任意大于1的值都行，取min值是因为我想看下能否按照Floyed算法思路计算最短路径
						if (graph[prev][next] == 0)
							graph[prev][next] = graph[prev][cur] + 1;
						else
							graph[prev][next] = std::min(graph[prev][next], graph[prev][cur] + 1);
					}
				}
				// 深度优先搜索进行拓扑排序
				if (--indegrees[next] == 0)
					dfsTopologicOrdering(graph, indegrees, next);
			}
		}
	}
};
```