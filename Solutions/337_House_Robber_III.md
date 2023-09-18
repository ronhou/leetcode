# 树形动态规划
此题仍然是 [198. House Robber](https://leetcode.cn/problems/house-robber/) 的进阶版：树形DP。参照上一题的思路，采取DFS的方式，计算偷或不偷根结点时最大价值即可。
+ 偷根结点时，一定不能偷子节点
+ 不偷根结点时，可以偷子节点，但是也可以不偷子节点，取较大值即可

同理，此题也可使类似上一题进行空间优化，在DFS的过程中，将子树偷与不偷的价值都计算出来，返回给根结点。每个结点都只会遍历到一次，因此不需要对价值表进行缓存。

```C++ [树形DP]
class Solution
{
public:
	int rob(TreeNode *root)
	{
		m_robbed.clear();
		m_notRobbed.clear();
		return std::max(dfsRob(root, true), dfsRob(root, false));
	}

	int dfsRob(TreeNode *root, bool robbed)
	{
		if (root == nullptr)
			return 0;
		if (robbed && m_robbed.count(root) > 0)
			return m_robbed[root];
		if (!robbed && m_notRobbed.count(root) > 0)
			return m_notRobbed[root];

		if (robbed)
		{
			int leftRobbed = dfsRob(root->left, false);
			int rightRobbed = dfsRob(root->right, false);
			m_robbed[root] = root->val + leftRobbed + rightRobbed;
			return m_robbed[root];
		}
		else
		{
			int leftRobbed = std::max(dfsRob(root->left, true), dfsRob(root->left, false));
			int rightRobbed = std::max(dfsRob(root->right, true), dfsRob(root->right, false));
			m_notRobbed[root] = leftRobbed + rightRobbed;
			return m_notRobbed[root];
		}
	}

private:
	std::unordered_map<TreeNode *, int> m_robbed;
	std::unordered_map<TreeNode *, int> m_notRobbed;
};
```
```C++ [空间优化]
struct RobResult
{
	int robbed;
	int notRobbed;
};

class Solution
{
public:
	int rob(TreeNode *root)
	{
		const RobResult &result = dfsRob(root);
		return std::max(result.robbed, result.notRobbed);
	}

	RobResult dfsRob(TreeNode *root)
	{
		if (root == nullptr)
			return {0, 0};

		const RobResult &leftResult = dfsRob(root->left);
		const RobResult &rightResult = dfsRob(root->right);
		return {
			root->val + leftResult.notRobbed + rightResult.notRobbed,
			std::max(leftResult.robbed, leftResult.notRobbed) + std::max(rightResult.robbed, rightResult.notRobbed),
		};
	}
};
```