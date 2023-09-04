# 模拟
利用矩阵本身的行列来记录信息，对有数字0的行列进行标记
```
void setMatrixZeroes(vector<vector<int>> &matrix)
{
	// 查找第一个有数字0的行，利用该行记录有数字0的列
	int row = -1;
	for (int i = 0; i < matrix.size(); i++)
	{
		bool hasZero = false;
		for (int j = 0; j < matrix[i].size(); j++)
		{
			if (matrix[i][j] == 0)
			{
				hasZero = true;
				break;
			}
		}
		if (hasZero)
		{
			row = i;
			break;
		}
	}
	// 没有数字0，直接退出
	if (row < 0)
	{
		return;
	}

	// 记录有数字0的列
	for (int j = 0; j < matrix[0].size(); j++)
	{
		for (int i = 0; i < matrix.size(); i++)
		{
			// 第j列有数字0，将该信息记录在第row行
			if (matrix[i][j] == 0)
			{
				matrix[row][j] = 0;
				break;
			}
		}
	}

	// 设置第row行以外的行
	for (int i = row + 1; i < matrix.size(); i++)
	{
		bool hasZero = false;
		for (int j = 0; j < matrix[i].size(); j++)
		{
			if (matrix[i][j] == 0)
			{
				hasZero = true;
				break;
			}
		}
		if (hasZero)
		{
			for (int j = 0; j < matrix[i].size(); j++)
			{
				matrix[i][j] = 0;
			}
		}
	}
	// 根据第row行记录的信息，将有数字0的列设置为0
	for (int j = 0; j < matrix[row].size(); j++)
	{
		if (matrix[row][j] == 0)
		{
			for (int i = 0; i < matrix.size(); i++)
			{
				matrix[i][j] = 0;
			}
		}
		// 将第row行全部设置为0
		matrix[row][j] = 0;
	}
}

class Solution
{
public:
	void setZeroes(vector<vector<int>> &matrix)
	{
		setMatrixZeroes(matrix);
	}
};
```