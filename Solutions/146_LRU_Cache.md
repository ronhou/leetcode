# 哈希表+双向链表
+ `get`和`put`时需要将数据更新到最新
+ `put`时，如果`size > capacity`需要删除最旧的一个数据
```C++
struct DLinkNode
{
	DLinkNode *prev, *next;
	int key, val;

	DLinkNode()
	{
		key = val = 0;
		prev = next = nullptr;
	}

	DLinkNode(int _key, int _val, DLinkNode *_prev)
	{
		key = _key;
		val = _val;
		prev = _prev;
		next = _prev->next;
	}
};

class LRUCache
{
public:
	LRUCache(int capacity)
	{
		m_cap = capacity;
		m_size = 0;
		m_head = new DLinkNode();
		m_tail = new DLinkNode();
		m_head->next = m_tail;
		m_tail->prev = m_head;
	}

	~LRUCache()
	{
		DLinkNode *node = m_head;
		while (node != nullptr)
		{
			DLinkNode *next = node->next;
			delete node;
			node = next;
		}
	}

	int get(int key)
	{
		if (m_nodeMap.find(key) == m_nodeMap.end())
			return -1;
		moveNodeToHead(key);
		return m_nodeMap[key]->val;
	}

	void put(int key, int value)
	{
		if (m_nodeMap.find(key) != m_nodeMap.end())
		{
			moveNodeToHead(key);
			m_nodeMap[key]->val = value;
			return;
		}

		if (m_size >= m_cap)
			removeLastNode();
		DLinkNode *node = new DLinkNode(key, value, m_head);
		m_head->next->prev = node;
		m_head->next = node;
		m_nodeMap[key] = node;
		++m_size;
	}

private:
	// 将node移动到双向链表头
	void moveNodeToHead(int key)
	{
		DLinkNode *node = m_nodeMap[key];
		if (node != nullptr)
		{
			// 将node从双向链表中拆出来
			node->prev->next = node->next;
			node->next->prev = node->prev;
			// 将node放入双向链表头
			node->next = m_head->next;
			node->prev = m_head;
			node->next->prev = node->prev->next = node;
		}
	}

	// 删除双向链表最后一个结点
	void removeLastNode()
	{
		DLinkNode *lastNode = m_tail->prev;
		if (lastNode != m_head)
		{
			m_tail->prev = lastNode->prev;
			m_tail->prev->next = m_tail;
			m_nodeMap.erase(lastNode->key);
			delete lastNode;
		}
	}

private:
	std::unordered_map<int, DLinkNode *> m_nodeMap;
	DLinkNode *m_head, *m_tail;
	int m_cap, m_size;
};

```