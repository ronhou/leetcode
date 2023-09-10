# 状态压缩后进行广度优先搜索
+ 将矩阵看作是一个十进制的9位数进行状态压缩
+ 使用BFS的思想，选择一个大于1的数字进行移动
+ 直到最终状态111111111

```C++
class Solution
{
public:
	int minimumMoves(std::vector<std::vector<int>> &grid)
	{
		int num = 0, moves = 0;
		const int rowSize = grid.size(), columnSize = grid[0].size(), gridSize = grid.size() * grid[0].size();
		for (int i = 0, k = 0; i < rowSize; i++)
		{
			for (int j = 0; j < columnSize; j++, k++)
				num = num * 10 + grid[i][j];
		}
		if (num == 111111111)
			return 0;

		const std::vector<std::pair<int, int>> dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
		std::vector<int> powers;
		powers.push_back(1);
		for (int i = 1; i < gridSize; i++)
			powers.push_back(powers.back() * 10);

		std::queue<std::pair<int, int>> q;
		q.push({num, 0});
		std::unordered_set<int> visited;
		visited.insert(num);
		while (!q.empty())
		{
			num = q.front().first;
			moves = q.front().second;
			q.pop();

			// 选一个数字大于1的
			for (int i = 0; i < gridSize; i++)
			{
				if (num / powers[i] % 10 > 1)
				{
					for (auto &&dir : dirs)
					{
						int x = i / columnSize + dir.first, y = i % columnSize + dir.second;
						if (0 <= x && x < rowSize && 0 <= y && y < columnSize)
						{
							int newNum = num - powers[i] + powers[x * rowSize + y];
							if (visited.count(newNum) == 0)
							{
								if (newNum == 111111111)
									return moves + 1;
								visited.insert(newNum);
								q.push({newNum, moves + 1});
							}
						}
					}
				}
			}
		}
		return 0;
	}
};
```