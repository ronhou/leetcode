# 递归
通过前序遍历和中序遍历构建二叉树
+ 前序遍历的顺序是：根左右；中序遍历的顺序是：左根右
+ 前序遍历的第一个元素即为根，查找其在中序遍历的位置index
+ 那么在中序遍历中index的左边即为左子树内容，index的右边即为右子树内容
+ 递归生成左子树和右子树
```C++
TreeNode *buildSubTree(std::vector<int> &preorder, int preBeginIndex, int preEndIndex, std::vector<int> &inorder, int inBeginIndex, int inEndIndex)
{
	int rootValue = preorder[preBeginIndex];
	int index = inBeginIndex;
	while (inorder[index] != rootValue)
		++index;

	int leftSubTreeSize = index - inBeginIndex;
	TreeNode *leftNode = nullptr;
	if (leftSubTreeSize > 0)
	{
		leftNode = buildSubTree(preorder, preBeginIndex + 1, preBeginIndex + leftSubTreeSize, inorder, inBeginIndex, index - 1);
	}
	TreeNode *rightNode = nullptr;
	if (index < inEndIndex)
	{
		rightNode = buildSubTree(preorder, preBeginIndex + leftSubTreeSize + 1, preEndIndex, inorder, index + 1, inEndIndex);
	}
	return new TreeNode(rootValue, leftNode, rightNode);
}

TreeNode *buildTreeByPreorderAndInorder(std::vector<int> &preorder, std::vector<int> &inorder)
{
	return buildSubTree(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1);
}
```