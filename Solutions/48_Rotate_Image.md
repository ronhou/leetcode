# 模拟
顺时针旋转矩阵，逆时针赋值
```C++ []
void clockwiseRotate(std::vector<std::vector<int>> &matrix)
{
	const int n = matrix.size();
	for (int x = 0; x < n / 2; x++)
	{
		for (int y = x; y < n - x - 1; y++)
		{
			// 逆时针赋值
			int temp = matrix[x][y];
			matrix[x][y] = matrix[n - y - 1][x];
			matrix[n - y - 1][x] = matrix[n - x - 1][n - y - 1];
			matrix[n - x - 1][n - y - 1] = matrix[y][n - x - 1];
			matrix[y][n - x - 1] = temp;
		}
	}
}
```