# DFS
使用深度优先搜索的方式查找下一个字符。搜索的过程中，对使用过的位置进行标记，下一个位置需等于word单词的下一个字符。

```C++
class Solution
{
public:
	bool exist(std::vector<std::vector<char>> &board, std::string word)
	{
		const std::vector<std::pair<int, int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
		std::vector<std::vector<bool>> used(board.size(), std::vector<bool>(board[0].size(), false));
		for (int i = 0; i < board.size(); i++)
		{
			for (int j = 0; j < board[i].size(); j++)
			{
				if (board[i][j] == word[0])
				{
					used[i][j] = true;
					if (dfsWord(board, word, used, dirs, i, j, 1))
						return true;
					used[i][j] = false;
				}
			}
		}
		return false;
	}

private:
	bool dfsWord(std::vector<std::vector<char>> &board, const std::string &word, std::vector<std::vector<bool>> &used, const std::vector<std::pair<int, int>> &dirs, int row, int col, int index)
	{
		if (index >= word.size())
			return true;

		for (auto &&dir : dirs)
		{
			int x = row + dir.first, y = col + dir.second;
			if (0 <= x && x < board.size() && 0 <= y && y < board[x].size() && !used[x][y] && board[x][y] == word[index])
			{
				used[x][y] = true;
				if (dfsWord(board, word, used, dirs, x, y, index + 1))
					return true;
				used[x][y] = false;
			}
		}
		return false;
	}
};
```