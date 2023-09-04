# 递归
+ 遇到叶子结点时，累加数字和
+ 否则将当前数字追加到上一层传下来的数字，递归传到下一层
```C++
void sumRootToLeafNumbers(TreeNode *root, int num, int& sum)
{
	num = num * 10 + root->val;
	if (root->left == nullptr && root->right == nullptr)
	{
		sum += num;
		return;
	}

	if (root->left != nullptr)
		sumRootToLeafNumbers(root->left, num, sum);
	if (root->right != nullptr)
		sumRootToLeafNumbers(root->right, num, sum);
}
```