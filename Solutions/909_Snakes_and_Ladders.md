# 广度优先搜索

```C++
int bfsCountMovesToReachTheEnd(std::vector<std::vector<int>> &board)
{
	const int m = board.size(), n = board[0].size(), squareSize = m * n;
	const int maxStep = 6;
	std::queue<std::pair<int, int>> q;
	q.push({0, 0});
	std::vector<bool> reached(squareSize, false);
	reached[0] = true;
	while (!q.empty())
	{
		std::pair<int, int> curr = q.front();
		q.pop();
		int upper = std::min(curr.first + maxStep, squareSize - 1);
		for (int next = curr.first + 1; next <= upper; next++)
		{
			int row = m - next / n - 1;
			int col = (next / n % 2 == 0) ? (next % n) : (n - next % n - 1);
			int nextIndex = (board[row][col] != -1) ? (board[row][col] - 1) : next;
			if (!reached[nextIndex])
			{
				if (nextIndex == squareSize - 1)
					return curr.second + 1;
				reached[nextIndex] = true;
				q.push({nextIndex, curr.second + 1});
			}
		}
	}

	return -1;
}
```