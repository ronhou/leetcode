## 递归实现中序遍历扁平化BST
+ 保存中序遍历的结果到数组，和当前迭代器所指向的索引
+ 通过判断数组索引，以及从数组取值的方式实现BST迭代器
```C++
class BSTIterator
{
public:
	BSTIterator(TreeNode *root)
	{
		m_index = 0;
		inorderTraversal(root);
	}

	int next()
	{
		return m_values[m_index++];
	}

	bool hasNext()
	{
		return m_index < m_values.size();
	}

private:
	void inorderTraversal(TreeNode *node)
	{
		if (node == nullptr)
			return;

		inorderTraversal(node->left);
		m_values.push_back(node->val);
		inorderTraversal(node->right);
	}

private:
	std::vector<int> m_values;
	int m_index;
};
```
---
> 题目最后要求O(1)的时间复杂度和O(h)的空间复杂度实现`next()`和`hasNext()`
## 使用栈模拟递归进行迭代
```C++
class BSTIterator
{
public:
	BSTIterator(TreeNode *root)
	{
		m_curNode = root;
	}

	int next()
	{
		while (m_curNode != nullptr)
		{
			m_nodeStack.push(m_curNode);
			m_curNode = m_curNode->left;
		}
		m_curNode = m_nodeStack.top();
		m_nodeStack.pop();

		int val = m_curNode->val;
		m_curNode = m_curNode->right;
		return val;
	}

	bool hasNext()
	{
		return m_curNode != nullptr || !m_nodeStack.empty();
	}

private:
	std::stack<TreeNode*> m_nodeStack;
	TreeNode* m_curNode;
};
```