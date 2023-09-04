> 第 112 场双周赛，第2题（#2840）：  
> [2840. Check if Strings Can be Made Equal With Operations II
](https://leetcode.cn/contest/biweekly-contest-112/problems/check-if-strings-can-be-made-equal-with-operations-ii/)

# 使用哈希表计数并比较
分别统计s1和s2奇数位置和偶数位置上各字符出现的次数，比较次数是否相等，不相等则无法变换
```C++
class Solution
{
public:
	bool checkStrings(string s1, string s2)
	{
		if (s1.size() != s2.size())
			return false;

		std::map<char, int> charCounts1, charCounts2;
		for (int i = 0; i < s1.size(); i += 2)
		{
			++charCounts1[s1[i]];
			++charCounts2[s2[i]];
		}
		auto it1 = charCounts1.begin();
		auto it2 = charCounts2.begin();
		for (; it1 != charCounts1.end() && it2 != charCounts2.end(); ++it1, ++it2)
		{
			if (it1->first != it2->first || it1->second != it2->second)
				return false;
		}

		charCounts1.clear();
		charCounts2.clear();
		for (int i = 1; i < s1.size(); i += 2)
		{
			++charCounts1[s1[i]];
			++charCounts2[s2[i]];
		}
		it1 = charCounts1.begin();
		it2 = charCounts2.begin();
		for (; it1 != charCounts1.end() && it2 != charCounts2.end(); ++it1, ++it2)
		{
			if (it1->first != it2->first || it1->second != it2->second)
				return false;
		}

		return true;
	}
};
```