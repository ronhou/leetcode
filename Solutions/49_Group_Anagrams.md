# 哈希表+排序/计数
``` C++ [哈希表+排序]
class Solution
{
public:
	std::vector<std::vector<std::string>> groupAnagrams(std::vector<std::string> &strs)
	{
		std::unordered_map<std::string, std::vector<string>> anagramMap;
		for (const std::string &str : strs)
		{
			string word = str;
			std::sort(word.begin(), word.end());
			anagramMap[word].emplace_back(str);
		}

		std::vector<std::vector<std::string>> anagrams;
		for (auto &anagram : anagramMap)
		{
			anagrams.emplace_back(anagram.second);
		}
		return anagrams;
	}
};
```
```C++ [无序哈希表+有序哈希表+字符计数]
class Solution
{
public:
	std::vector<std::vector<std::string>> groupAnagrams(std::vector<std::string> &strs)
	{
		std::unordered_map<std::string, std::vector<std::string>> anagramMap;
		for (const std::string &str : strs)
		{
			std::map<char, int> charCounts;
			for (char c : str)
			{
				charCounts[c]++;
			}

			std::string key;
			for (auto &charCountPair : charCounts)
			{
				key += charCountPair.first + std::to_string(charCountPair.second);
			}
			anagramMap[key].emplace_back(str);
		}

		std::vector<std::vector<std::string>> anagrams;
		for (auto &anagram : anagramMap)
		{
			anagrams.emplace_back(anagram.second);
		}
		return anagrams;
	}
};
```