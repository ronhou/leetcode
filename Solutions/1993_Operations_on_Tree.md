> [1993. Operations on Tree](https://leetcode.cn/problems/operations-on-tree/)

## 深度优先搜索
添加`LockingTreeNode`类，构建`LockingTree`，保存`num`到`LockingTreeNode`的映射，使用`DFS`处理`upgrade`。

```C++
class LockingTree
{
private:
	class LockingTreeNode
	{
	public:
		LockingTreeNode()
		{
			m_lockedUser = 0;
			m_parent = nullptr;
		}

		bool lock(int user)
		{
			if (isLocked())
				return false;
			m_lockedUser = user;
			return true;
		}

		bool unlock(int user = -1)
		{
			if (user > 0 && m_lockedUser != user)
				return false;
			m_lockedUser = 0;
			return true;
		}

		bool isLocked()
		{
			return m_lockedUser > 0;
		}

		bool upgrade(int user)
		{
			if (isLocked())
				return false;

			LockingTreeNode *parent = m_parent;
			while (parent != nullptr)
			{
				if (parent->isLocked())
					return false;
				parent = parent->m_parent;
			}

			if (unlockDescendants(this))
				return this->lock(user);
			return false;
		}

		void setParent(LockingTreeNode *parent)
		{
			m_parent = parent;
			parent->m_children.push_back(this);
		}

		void addChild(LockingTreeNode *child)
		{
			m_children.push_back(child);
			child->m_parent = this;
		}

	private:
		bool unlockDescendants(LockingTreeNode *node)
		{
			bool ret = false;
			if (node->isLocked())
			{
				node->unlock();
				ret = true;
			}

			for (auto &&child : node->m_children)
			{
				if (unlockDescendants(child))
					ret = true;
			}
			return ret;
		}

	private:
		LockingTreeNode *m_parent;
		std::vector<LockingTreeNode *> m_children;
		int m_lockedUser;
	};

public:
	LockingTree(std::vector<int> &parent)
	{
		buildTree(parent);
	}

	~LockingTree()
	{
		for (size_t i = 0; i < m_nodes.size(); i++)
		{
			delete m_nodes[i];
			m_nodes[i] = nullptr;
		}
		m_nodes.clear();
	}

	bool lock(int num, int user)
	{
		return m_nodes[num]->lock(user);
	}

	bool unlock(int num, int user)
	{
		return m_nodes[num]->unlock(user);
	}

	bool upgrade(int num, int user)
	{
		return m_nodes[num]->upgrade(user);
	}

private:
	void buildTree(std::vector<int> &parents)
	{
		m_nodes.resize(parents.size());
		for (size_t i = 0; i < parents.size(); i++)
			m_nodes[i] = new LockingTreeNode();
		for (size_t i = 0; i < parents.size(); i++)
		{
			if (parents[i] >= 0 && parents[i] < parents.size())
				m_nodes[i]->setParent(m_nodes[parents[i]]);
		}
	}

private:
	std::vector<LockingTreeNode *> m_nodes;
};
```