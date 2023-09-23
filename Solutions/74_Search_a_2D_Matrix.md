> [74. Search a 2D Matrix](https://leetcode.cn/problems/search-a-2d-matrix/)
# 二分查找

```C++
class Solution
{
public:
	bool searchMatrix(std::vector<std::vector<int>> &matrix, int target)
	{
		const size_t rowSize = matrix.size(), columnSize = matrix.front().size();
		int l = 0, r = rowSize * columnSize - 1, mid = 0;
		while (l < r)
		{
			mid = (l + r) / 2;
			if (matrix[mid / columnSize][mid % columnSize] < target)
				l = mid + 1;
			else
				r = mid;
		}
		return matrix[l / columnSize][l % columnSize] == target;
	}
};
```
```C++
class Solution
{
public:
	bool searchMatrix(std::vector<std::vector<int>> &matrix, int target)
	{
		const size_t rowSize = matrix.size(), columnSize = matrix.front().size();
		int l = 0, r = rowSize * columnSize - 1, mid = 0, val = 0;
		while (l <= r)
		{
			mid = (l + r) / 2;
			val = matrix[mid / columnSize][mid % columnSize];
			if (val == target)
				return true;
			if (val < target)
				l = mid + 1;
			else
				r = mid - 1;
		}
		return false;
	}
};
```
```C++
class Solution
{
public:
	bool searchMatrix(std::vector<std::vector<int>> &matrix, int target)
	{
		if (target < matrix.front().front())
			return false;
		auto itRow = std::upper_bound(matrix.begin(), matrix.end(), target, [](const int val, const std::vector<int>& mid) -> bool {
			return val < mid.front();
		});
		--itRow;
		auto it = std::lower_bound(itRow->begin(), itRow->end(), target);
		return it != itRow->end() && *it == target;
		// return std::binary_search(itRow->begin(), itRow->end(), target);
	}
};
```