# 滑动窗口
```C++ []
/*
作者：力扣官方题解
链接：https://leetcode.cn/problems/substring-with-concatenation-of-all-words/solutions/1616997/chuan-lian-suo-you-dan-ci-de-zi-chuan-by-244a/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
*/
std::vector<int> findWordsPermutationStartIndices(std::string &s, std::vector<std::string> &words)
{
	std::vector<int> startIndices;
	int wordsSize = words.size(), wordSize = words[0].size(), len = s.size();
	for (int i = 0; i < wordSize && i + wordsSize * wordSize <= len; i++)
	{
		std::unordered_map<string, int> wordCounts;
		for (int j = 0; j < wordsSize; j++)
		{
			wordCounts[s.substr(i + j * wordSize, wordSize)]++;
		}
		for (const string &word : words)
		{
			wordCounts[word]--;
			if (wordCounts[word] == 0)
			{
				wordCounts.erase(word);
			}
		}
		if (wordCounts.empty())
		{
			startIndices.push_back(i);
		}

		for (int j = i + wordSize; j + wordsSize * wordSize <= len; j += wordSize)
		{
			string word = s.substr(j - wordSize, wordSize);
			if (--wordCounts[word] == 0)
			{
				wordCounts.erase(word);
			}
			word = s.substr(j + (wordsSize - 1) * wordSize, wordSize);
			if (++wordCounts[word] == 0)
			{
				wordCounts.erase(word);
			}
			if (wordCounts.empty())
			{
				startIndices.push_back(j);
			}
		}
	}
	return startIndices;
}
```