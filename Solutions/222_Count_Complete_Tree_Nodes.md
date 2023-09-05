## 递归
```C++
int countCompleteBinaryTreeNodes(TreeNode *root)
{
	if (root == nullptr)
		return 0;
	return countCompleteBinaryTreeNodes(root->left) + countCompleteBinaryTreeNodes(root->right) + 1;
}
```

题目进阶要求
> 进阶：遍历树来统计节点是一种时间复杂度为`O(n)`的简单解决方案。你可以设计一个更快的算法吗？  
> 官方题解：[二分查找 + 位运算](https://leetcode.cn/problems/count-complete-tree-nodes/solutions/495655/wan-quan-er-cha-shu-de-jie-dian-ge-shu-by-leetco-2/)