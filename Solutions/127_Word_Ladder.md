# 广度优先搜索

```C++
int minTransformSequence(std::string beginWord, std::string endWord, std::vector<std::string> &wordList)
{
	if (beginWord == endWord)
		return 1;

	// 存放转换到目标单词所需的次数
	std::unordered_map<std::string, int> wordTransLengths;
	for (auto &&word : wordList)
		wordTransLengths[word] = 0;
	wordTransLengths[beginWord] = 1;

	std::queue<std::string> q;
	q.push(beginWord);
	while (!q.empty())
	{
		std::string currWord = q.front();
		q.pop();
		int length = wordTransLengths[currWord];
		for (int i = 0; i < currWord.size(); i++)
		{
			const char currChar = currWord[i];
			for (char c = 'a'; c <= 'z'; c++)
			{
				if (c != currChar)
				{
					currWord[i] = c;
					if (wordTransLengths.count(currWord) > 0 && wordTransLengths[currWord] == 0)
					{
						if (currWord == endWord)
							return length + 1;
						wordTransLengths[currWord] = length + 1;
						q.push(currWord);
					}
				}
			}
			currWord[i] = currChar;
		}
	}
	return 0;
}
```