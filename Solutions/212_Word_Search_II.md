# 字典树 + 回溯
使用单词列表`words`构建字典树，然后采用回溯的方式，判断网格`board`中存在与字典树上的单词。
递归过程中，字典树结点的指针跟随前进。
但是，会超时。

```C++
class Trie
{
public:
	Trie(char val = '\0', bool isEnd = false)
	{
		m_val = val;
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

	void addWord(const std::string &word)
	{
		Trie *node = this;
		for (auto &&c : word)
		{
			if (node->m_children.count(c) == 0)
				node->m_children[c] = new Trie(c);
			node = node->m_children[c];
		}
		node->m_isEnd = true;
	}

	Trie *getChild(char c)
	{
		if (m_children.count(c) == 0)
			return nullptr;
		return m_children[c];
	}

	bool isEnd()
	{
		return m_isEnd;
	}

private:
	char m_val;
	bool m_isEnd;
	std::unordered_map<char, Trie *> m_children;
};

class Solution
{
public:
	std::vector<std::string> findWords(std::vector<std::vector<char>> &board, std::vector<std::string> &words)
	{
		Trie trie;
		for (auto &&word : words)
			trie.addWord(word);

		const std::vector<std::pair<int, int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
		std::vector<std::vector<bool>> used(board.size(), std::vector<bool>(board[0].size(), false));
		std::unordered_set<std::string> matches;
		std::string word;
		for (int i = 0; i < board.size(); i++)
		{
			for (int j = 0; j < board[i].size(); j++)
				dfs(board, used, dirs, word, i, j, &trie, matches);
		}
		return std::vector<std::string>(matches.begin(), matches.end());
	}

	void dfs(const std::vector<std::vector<char>> &board,
			 std::vector<std::vector<bool>> &used,
			 const std::vector<std::pair<int, int>> &dirs,
			 std::string &word,
			 int row,
			 int col,
			 Trie *trieNode,
			 std::unordered_set<std::string> &matches)
	{
		Trie *child = trieNode->getChild(board[row][col]);
		if (child == nullptr)
			return;

		word.push_back(board[row][col]);
		used[row][col] = true;
		if (child->isEnd())
			matches.insert(word);

		for (auto &&dir : dirs)
		{
			int x = row + dir.first, y = col + dir.second;
			if (0 <= x && x < board.size() && 0 <= y && y < board[x].size() && !used[x][y])
				dfs(board, used, dirs, word, x, y, child, matches);
		}

		word.pop_back();
		used[row][col] = false;
	}
};
```

根据[力扣官方题解](https://leetcode.cn/problems/word-search-ii/solutions/1000172/dan-ci-sou-suo-ii-by-leetcode-solution-7494/)方法二进行优化。
```C++
void dfs(const std::vector<std::vector<char>> &board,
		 std::vector<std::vector<bool>> &used,
		 const std::vector<std::pair<int, int>> &dirs,
		 std::string &word,
		 int row,
		 int col,
		 Trie *trieNode,
		 std::unordered_set<std::string> &matches)
{
	Trie *child = trieNode->getChild(board[row][col]);
	if (child == nullptr)
		return;

	word.push_back(board[row][col]);
	used[row][col] = true;
	if (child->isEnd())
		matches.insert(word);

	for (auto &&dir : dirs)
	{
		int x = row + dir.first, y = col + dir.second;
		if (0 <= x && x < board.size() && 0 <= y && y < board[x].size() && !used[x][y])
			dfs(board, used, dirs, word, x, y, child, matches);
	}

	if (child->childrenEmpty())
		trieNode->eraseChild(board[row][col]);
	word.pop_back();
	used[row][col] = false;
}
```