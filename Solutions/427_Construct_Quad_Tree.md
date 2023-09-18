# 分治
很典型的分治，使用递归的方式，将矩阵二分为更小的块，直到块的长度等于1，那么此时必定是叶子结点。如果四个孩子结点相等，那么当前结点也即叶子结点。

```C++
class Solution
{
public:
	Node *construct(std::vector<std::vector<int>> &grid)
	{
		return constructQuadTree(grid, 0, grid.size() - 1, 0, grid.size() - 1);
	}

	Node *constructQuadTree(const std::vector<std::vector<int>> &grid, int beginRow, int endRow, int beginColumn, int endColumn)
	{
		if (beginRow >= endRow)
			return new Node(!!grid[beginRow][beginColumn], true);

		int centerRow = (beginRow + endRow) / 2, centerColumn = (beginColumn + endColumn) / 2;
		Node *topLeftNode = constructQuadTree(grid, beginRow, centerRow, beginColumn, centerColumn);
		Node *topRightNode = constructQuadTree(grid, beginRow, centerRow, centerColumn + 1, endColumn);
		Node *bottomLeftNode = constructQuadTree(grid, centerRow + 1, endRow, beginColumn, centerColumn);
		Node *bottomRightNode = constructQuadTree(grid, centerRow + 1, endRow, centerColumn + 1, endColumn);
		if (topLeftNode->isLeaf &&
			topRightNode->isLeaf &&
			bottomLeftNode->isLeaf &&
			bottomRightNode->isLeaf &&
			topLeftNode->val == topRightNode->val &&
			topLeftNode->val == bottomLeftNode->val &&
			topLeftNode->val == bottomRightNode->val)
		{
			bool val = topLeftNode->val;
			delete topLeftNode;
			delete topRightNode;
			delete bottomLeftNode;
			delete bottomRightNode;
			return new Node(val, true);
		}

		return new Node(true, false, topLeftNode, topRightNode, bottomLeftNode, bottomRightNode);
	}
};
```