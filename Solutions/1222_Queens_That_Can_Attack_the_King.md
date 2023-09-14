# 模拟
1. 使用`std::set<std::pair<int, int>>`标记王后的位置
   + 没有使用`std::unordered_set<std::pair<int, int>>`，因为要需要自己写哈希函数
2. 从国王的位置朝8个方向进行扩展，遇到的首个王后即“可以攻击国王的王后”

```C++
class Solution
{
public:
	std::vector<std::vector<int>> queensAttacktheKing(std::vector<std::vector<int>> &queens, std::vector<int> &king)
	{
		std::set<std::pair<int, int>> queenSet;
		for (auto &&queen : queens)
			queenSet.insert({queen[0], queen[1]});

		const int maxRow = 8, maxCol = 8;
		const std::vector<std::pair<int, int>> dirs = {{-1, 0}, {-1, 1}, {0, 1}, {1, 1}, {1, 0}, {1, -1}, {0, -1}, {-1, -1}};
		std::vector<std::vector<int>> coords;
		for (auto &&dir : dirs)
		{
			std::pair<int, int> coord = {king[0] + dir.first, king[1] + dir.second};
			while (0 <= coord.first && coord.first < maxRow && 0 <= coord.second && coord.second < maxCol && queenSet.count(coord) == 0)
			{
				coord.first += dir.first;
				coord.second += dir.second;
			}
			if (queenSet.count(coord) > 0)
				coords.push_back({coord.first, coord.second});
		}
		return coords;
	}
};
```