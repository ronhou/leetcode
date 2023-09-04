# 递归
通过后序遍历和中序遍历构建二叉树
+ 后序遍历的顺序是：左右根；中序遍历的顺序是：左根右
+ 后序遍历的最后一个元素即为根，查找其在中序遍历的位置index
+ 那么在中序遍历中index的左边即为左子树内容，index的右边即为右子树内容
+ 递归生成左子树和右子树
```C++
TreeNode *buildSubTree(std::vector<int> &postorder, int postBeginIndex, int postEndIndex, std::vector<int> &inorder, int inBeginIndex, int inEndIndex)
{
	int rootValue = postorder[postEndIndex];
	int index = inBeginIndex;
	while (inorder[index] != rootValue)
		++index;

	int leftSubTreeSize = index - inBeginIndex;
	TreeNode *leftNode = nullptr;
	if (leftSubTreeSize > 0)
	{
		leftNode = buildSubTree(postorder, postBeginIndex, postBeginIndex + leftSubTreeSize - 1, inorder, inBeginIndex, index - 1);
	}
	TreeNode *rightNode = nullptr;
	if (index < inEndIndex)
	{
		rightNode = buildSubTree(postorder, postBeginIndex + leftSubTreeSize, postEndIndex - 1, inorder, index + 1, inEndIndex);
	}
	return new TreeNode(rootValue, leftNode, rightNode);
}

TreeNode *buildTreeByPostorderAndInorder(std::vector<int> &postorder, std::vector<int> &inorder)
{
	return buildSubTree(postorder, 0, postorder.size() - 1, inorder, 0, inorder.size() - 1);
}
```