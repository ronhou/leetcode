## DFS/BFS
+ 以某块陆地（'1'）为起点进行泛洪遍历（DFS或BFS）
+ 遍历的次数即为岛屿的个数

```C++
void flood(const std::vector<std::vector<char>> &grid, std::vector<std::vector<bool>> &visited, const std::vector<std::pair<int, int>> &dirs, int x, int y)
{
	visited[x][y] = true;
	for (int i = 0; i < dirs.size(); i++)
	{
		int newX = x + dirs[i].first;
		int newY = y + dirs[i].second;
		if (0 <= newX && newX < grid.size() && 0 <= newY && newY < grid[x].size() && grid[newX][newY] == '1' && !visited[newX][newY])
			flood(grid, visited, dirs, newX, newY);
	}
}

int countIslands(const std::vector<std::vector<char>> &grid)
{
	const std::vector<std::pair<int, int>> dirs = {
		{-1, 0},
		{1, 0},
		{0, -1},
		{0, 1},
	};
	int count = 0;
	std::vector<std::vector<bool>> visited(grid.size(), std::vector<bool>(grid[0].size(), false));
	for (int i = 0; i < grid.size(); i++)
	{
		for (int j = 0; j < grid[i].size(); j++)
		{
			if (grid[i][j] == '1' && !visited[i][j])
			{
				++count;
				flood(grid, visited, dirs, i, j);
			}
		}
	}
	return count;
}
```