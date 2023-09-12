# Trie

```C++
class Trie
{
public:
	class TrieNode
	{
	public:
		TrieNode(char ch, bool isEnd)
		{
			m_char = ch;
			m_isEnd = isEnd;
		}
		~TrieNode()
		{
			for (auto &&child : m_children)
			{
				delete child.second;
				child.second = NULL;
			}
			m_children.clear();
		}

	public:
		char m_char;
		bool m_isEnd;
		std::unordered_map<char, TrieNode *> m_children;
	};

public:
	Trie()
	{
		m_root = new TrieNode('\0', false);
	}
	~Trie()
	{
		delete m_root;
		m_root = NULL;
	}

	void insert(string word)
	{
		TrieNode *node = m_root;
		for (auto &&ch : word)
		{
			if (node->m_children.count(ch) > 0)
			{
				node = node->m_children[ch];
			}
			else
			{
				TrieNode *child = new TrieNode(ch, false);
				node->m_children[ch] = child;
				node = child;
			}
		}
		node->m_isEnd = true;
	}

	bool search(string word)
	{
		TrieNode *node = m_root;
		for (auto &&ch : word)
		{
			if (node->m_children.count(ch) == 0)
				return false;
			else
				node = node->m_children[ch];
		}
		return node->m_isEnd;
	}

	bool startsWith(string prefix)
	{
		TrieNode *node = m_root;
		for (auto &&ch : prefix)
		{
			if (node->m_children.count(ch) == 0)
				return false;
			else
				node = node->m_children[ch];
		}
		return true;
	}

private:
	TrieNode *m_root;
};
```