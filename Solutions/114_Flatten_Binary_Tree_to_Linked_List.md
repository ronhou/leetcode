# 递归
+ 递归处理，返回链表的最后一个结点
  + 有右子树时，返回右子树形成的链表的最后一个结点
  + 否则，有左子树时，返回左子树形成的链表的最后一个结点
  + 否则，返回根结点
+ 处理当前根节点
  + 左子树的最后一个结点的next（right）指向当前结点的右孩子
  + 当前根结点next（right）指向当前结点的左孩子
  + 当前结点的左孩子置空

```C++
TreeNode *flattenTreeNodeByPreorder(TreeNode *root)
{
	if (root == nullptr)
		return nullptr;

	TreeNode *leftNode = flattenTreeNodeByPreorder(root->left);
	TreeNode *rightNode = flattenTreeNodeByPreorder(root->right);
	if (leftNode)
	{
		leftNode->right = root->right;
		root->right = root->left;
		root->left = nullptr;
	}

	return rightNode ? rightNode : (leftNode ? leftNode : root);
}
```