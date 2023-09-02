# 模拟
```C++ []
class Sudoku
{
public:
	static bool isValid(const std::vector<std::vector<char>> &board)
	{
		return isRowsValid(board) && isColumnsValid(board) && isBoxesValid(board);
	}

private:
	static bool isRowsValid(const std::vector<std::vector<char>> &board)
	{
		for (const vector<char> &row : board)
		{
			std::vector<bool> used(row.size(), false);
			for (char num : row)
			{
				if (num != '.')
				{
					if (used[num - '1'])
					{
						return false;
					}
					used[num - '1'] = true;
				}
			}
		}
		return true;
	}

	static bool isColumnsValid(const std::vector<std::vector<char>> &board)
	{
		for (int col = 0; col < board[0].size(); col++)
		{
			std::vector<bool> used(board.size(), false);
			for (int row = 0; row < board.size(); row++)
			{
				if (board[row][col] != '.')
				{
					if (used[board[row][col] - '1'])
					{
						return false;
					}
					used[board[row][col] - '1'] = true;
				}
			}
		}
		return true;
	}

	static bool isBoxesValid(const std::vector<std::vector<char>> &board)
	{
		for (int rowI = 0; rowI < 3; rowI++)
		{
			for (int colI = 0; colI < 3; colI++)
			{
				std::vector<bool> used(board.size(), false);
				for (int i = 0; i < 3; i++)
				{
					for (int j = 0; j < 3; j++)
					{
						int row = rowI * 3 + i;
						int col = colI * 3 + j;
						if (board[row][col] != '.')
						{
							if (used[board[row][col] - '1'])
							{
								return false;
							}
							used[board[row][col] - '1'] = true;
						}
					}
				}
			}
		}
		return true;
	}
};
```