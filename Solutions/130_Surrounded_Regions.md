# DFS/BFS
1. 从矩阵边界为'O'的位置进行遍历
2. 对所有遍历过的位置进行标记
3. 所有没有遍历过的'O'的位置即是需要被翻转的

```C++
void fill(const std::vector<std::vector<char>> &board, std::vector<std::vector<bool>> &travelled, const std::vector<std::pair<int, int>> &dirs, int row, int col)
{
	travelled[row][col] = true;
	for (auto &dir : dirs)
	{
		int x = row + dir.first, y = col + dir.second;
		if (0 <= x && x < board.size() && 0 <= y && y < board[row].size() && board[x][y] == 'O' && !travelled[x][y])
			fill(board, travelled, dirs, x, y);
	}
}

void flipSurroundedRegions(std::vector<std::vector<char>> &board)
{
	const std::vector<std::pair<int, int>> dirs = {
		{-1, 0},
		{1, 0},
		{0, -1},
		{0, 1},
	};
	std::vector<std::vector<bool>> travelled(board.size(), std::vector<bool>(board[0].size(), false));
	// 从边界位置进行遍历
	for (int i = 0; i < board.size(); i++)
	{
		if (board[i][0] == 'O' && !travelled[i][0])
			fill(board, travelled, dirs, i, 0);
		if (board[i][board[i].size() - 1] == 'O' && !travelled[i][board[i].size() - 1])
			fill(board, travelled, dirs, i, board[i].size() - 1);
	}
	for (int i = 1; i < board[0].size() - 1; i++)
	{
		if (board[0][i] == 'O' && !travelled[0][i])
			fill(board, travelled, dirs, 0, i);
		if (board[board.size() - 1][i] == 'O' && !travelled[board.size() - 1][i])
			fill(board, travelled, dirs, board.size() - 1, i);
	}

	// 没有被遍历到的'O'需要被翻转
	for (int i = 0; i < board.size(); i++)
	{
		for (int j = 0; j < board[i].size(); j++)
		{
			if (board[i][j] == 'O' && !travelled[i][j])
				board[i][j] = 'X';
		}
	}
}
```