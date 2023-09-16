# Trie

```C++
class Trie
{
public:
	Trie(char ch = '\0', bool isEnd = false)
	{
		m_char = ch;
		m_isEnd = isEnd;
	}
	~Trie()
	{
		for (auto &&child : m_children)
		{
			delete child.second;
			child.second = nullptr;
		}
		m_children.clear();
	}

	void insert(const std::string &word)
	{
		Trie *node = this;
		for (auto &&ch : word)
		{
			if (node->m_children.count(ch) > 0)
			{
				node = node->m_children[ch];
			}
			else
			{
				Trie *child = new Trie(ch);
				node->m_children[ch] = child;
				node = child;
			}
		}
		node->m_isEnd = true;
	}

	bool search(const string &word)
	{
		Trie *node = this;
		for (auto &&ch : word)
		{
			if (node->m_children.count(ch) == 0)
				return false;
			else
				node = node->m_children[ch];
		}
		return node->m_isEnd;
	}

	bool startsWith(std::string &prefix)
	{
		Trie *node = this;
		for (auto &&ch : prefix)
		{
			if (node->m_children.count(ch) == 0)
				return false;
			else
				node = node->m_children[ch];
		}
		return true;
	}

	Trie *getChild(char ch)
	{
		if (m_children.count(ch) == 0)
			return nullptr;
		else
			return m_children[ch];
	}

	bool isEnd()
	{
		return m_isEnd;
	}

	// 返回vector拷贝赋值会超时
	std::unordered_map<char, Trie *>& getChildren()
	{
		return m_children;
	}

private:
	char m_char;
	bool m_isEnd;
	std::unordered_map<char, Trie *> m_children;
};

class WordDictionary
{
public:
	WordDictionary()
	{
		m_trie = new Trie();
	}
	~WordDictionary()
	{
		delete m_trie;
		m_trie = nullptr;
	}

	void addWord(std::string word)
	{
		m_trie->insert(word);
	}

	bool search(std::string word)
	{
		return this->search(word, 0, m_trie);
	}

private:
	bool search(const std::string &word, int index, Trie *node)
	{
		if (index >= word.size())
			return node->isEnd();

		if (word[index] == '.')
		{
			std::unordered_map<char, Trie *>& children = node->getChildren();
			for (auto &&child : children)
			{
				if (this->search(word, index + 1, child.second))
					return true;
			}
			return false;
		}

		Trie *child = node->getChild(word[index]);
		if (child == nullptr)
			return false;
		return this->search(word, index + 1, child);
	}

private:
	Trie *m_trie;
};
```