# 模拟

```C++
class Solution
{
public:
	bool checkValidGrid(std::vector<std::vector<int>> &grid)
	{
		std::vector<std::pair<int, int>> positions(grid.size() * grid[0].size(), {-1, -1});
		for (int i = 0; i < grid.size(); i++)
		{
			for (int j = 0; j < grid[i].size(); j++)
			{
				int index = grid[i][j];
				if (positions[index].first != -1)
					return false;
				positions[index].first = i;
				positions[index].second = j;
			}
		}
		for (int i = 1; i < positions.size(); i++)
		{
			int rowDiff = std::abs(positions[i - 1].first - positions[i].first);
			int colDiff = std::abs(positions[i - 1].second - positions[i].second);
			if (!(rowDiff == 1 && colDiff == 2 || rowDiff == 2 && colDiff == 1))
				return false;
		}
		return true;
	}
};
```