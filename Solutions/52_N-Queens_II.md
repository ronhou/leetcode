# 递归回溯
+ 按行进行递归回溯处理，寻找该行能放置皇后的位置。
+ 使用布尔数组进行标记，标记列，对角线和反向对角线是否已被放置皇后
  + 行无需使用布尔数组进行标记，因为是按照行进行递归处理的

```C++
class Solution
{
public:
	int totalNQueens(int n)
	{
		std::vector<bool> columns(n, false), diagonal(n + n - 1, false), backDiagoal(n + n - 1, false);
		int count = 0;
		placingNQueens(n, 0, columns, diagonal, backDiagoal, count);
		return count;
	}

private:
	void placingNQueens(const int &n, int rowIndex, std::vector<bool> &columns, std::vector<bool> &diagonal, std::vector<bool> &backDiagonal, int &count)
	{
		if (rowIndex >= n)
		{
			++count;
			return;
		}
		for (int i = 0; i < n; i++)
		{
			if (!columns[i] && !diagonal[rowIndex + i] && !backDiagonal[rowIndex + n - i - 1])
			{
				columns[i] = diagonal[rowIndex + i] = backDiagonal[rowIndex + n - i - 1] = true;
				placingNQueens(n, rowIndex + 1, columns, diagonal, backDiagonal, count);
				columns[i] = diagonal[rowIndex + i] = backDiagonal[rowIndex + n - i - 1] = false;
			}
		}
	}
};
```