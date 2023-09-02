# 模拟
```C++ []
/*
const std::vector<std::pair<int, int>> clockwiseDirections = {
    {0, 1},
    {1, 0},
    {0, -1},
    {-1, 0},
};
*/
std::vector<int> spiralOrder(const std::vector<std::vector<int>> &matrix, const std::vector<std::pair<int, int>> &directions)
{
	std::vector<int> order;
	int m = matrix.size(), n = matrix[0].size(), matrixSize = matrix.size() * matrix[0].size();
	std::vector<std::vector<bool>> visit(m, std::vector<bool>(n, false));
	int x = 0, y = 0, dir = 0;
	do
	{
		order.push_back(matrix[x][y]);
		visit[x][y] = true;
		for (int i = 0; i < directions.size(); i++)
		{
			int newDir = (dir + i) % directions.size();
			int newX = x + directions[newDir].first;
			int newY = y + directions[newDir].second;
			if (0 <= newX && newX < m && 0 <= newY && newY < n && !visit[newX][newY])
			{
				x = newX;
				y = newY;
				dir = newDir;
				break;
			}
		}
	} while (order.size() < matrixSize);
	return order;
}
```