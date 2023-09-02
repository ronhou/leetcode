# 模拟
```C++
int getLiveNeighbors(const std::vector<std::vector<int>> &board, int row, int col)
{
	static const std::array<std::array<int, 2>, 8> neighborDirs = {
		{{{-1, -1}},
		 {{-1, 0}},
		 {{-1, 1}},
		 {{0, -1}},
		 {{0, 1}},
		 {{1, -1}},
		 {{1, 0}},
		 {{1, 1}}}};

	int neighborsCount = 0;
	for (int i = 0; i < neighborDirs.size(); i++)
	{
		int x = row + neighborDirs[i][0];
		int y = col + neighborDirs[i][1];
		if (0 <= x && x < board.size() && 0 <= y && y < board[row].size())
		{
			if (board[x][y] == 1 || board[x][y] == -1)
			{
				++neighborsCount;
			}
		}
	}
	return neighborsCount;
}

void nextStateOfGame(std::vector<std::vector<int>> &board)
{
	for (int i = 0; i < board.size(); i++)
	{
		for (int j = 0; j < board[i].size(); j++)
		{
			int count = getLiveNeighbors(board, i, j);
			if (board[i][j] > 0)
			{
				board[i][j] = (count < 2 || count > 3) ? -1 : 1;
			}
			else
			{
				nextBoard[i][j] = (count == 3) ? 2 : 0;
			}
		}
	}
	for (int i = 0; i < board.size(); i++)
	{
		for (int j = 0; j < board[i].size(); j++)
		{
			if (board[i][j] == -1)
			{
				board[i][j] = 0;
			}
			else if (board[i][j] == 2)
			{
				board[i][j] = 1;
			}
		}
	}
}
```