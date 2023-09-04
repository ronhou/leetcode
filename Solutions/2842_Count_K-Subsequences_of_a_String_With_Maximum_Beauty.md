> 第 112 场双周赛，第4题（#2842）：  
> [2842. Count K-Subsequences of a String With Maximum Beauty](https://leetcode.cn/contest/biweekly-contest-112/problems/count-k-subsequences-of-a-string-with-maximum-beauty/)

# 排序后贪心+计算组合数
1. 统计字符出现的次数，即f(c)
2. 对f(c)进行排序（从大到小排序更好处理）
3. 计算次数等于charCounts[k - 1]的字符的索引范围：[b, e]
4. 贪心：[0, b-1]字符必取，[b, e]字符从中任意取k-b个字符
5. 使用质因子分解的方式计算组合数：C(e-b+1, k-b)

```C++
class Solution
{
public:
	int countKSubsequencesWithMaxBeauty(std::string s, int k)
	{
		// 统计字符出现的次数，即f(c)
		std::unordered_map<char, int> charCountMap;
		for (char c : s)
			++charCountMap[c];
		if (charCountMap.size() < k)
			return 0;

		// 对f(c)进行排序（从大到小排序更好处理）
		std::vector<int> charCounts;
		for (auto it : charCountMap)
			charCounts.push_back(it.second);
		std::sort(charCounts.begin(), charCounts.end(), std::greater<int>());

		// 计算次数等于charCounts[k - 1]的字符的索引范围：[b, e]
		int b = k - 1, e = k - 1;
		while (b > 0 && charCounts[b - 1] == charCounts[k - 1])
			--b;
		while (e < charCounts.size() - 1 && charCounts[e + 1] == charCounts[k - 1])
			++e;

		// [0, b-1]字符必取，[b, e]字符从中任意取k-b个字符
		const int Mod = 1e9 + 7;
		long long ans = 1;
		for (int i = 0; i < b; i++)
			ans = ans * charCounts[i] % Mod;
		for (int i = b; i < k; i++)
			ans = ans * charCounts[k - 1] % Mod;

		// 质因子分解：计算组合数的质因子个数
		std::vector<int> primes = {2, 3, 5, 7, 11, 13, 17, 19, 23};
		std::unordered_map<int, int> primeCounts;
		for (int i = k - b + 1; i <= e - b + 1; i++)
		{
			int x = i;
			for (int j = 0; j < primes.size() && primes[j] <= x; j++)
			{
				while (x > 1 && x % primes[j] == 0)
				{
					++primeCounts[primes[j]];
					x /= primes[j];
				}
			}
		}
		for (int i = 2; i <= e - k + 1; i++)
		{
			int x = i;
			for (int j = 0; j < primes.size() && primes[j] <= x; j++)
			{
				while (x > 1 && x % primes[j] == 0)
				{
					--primeCounts[primes[j]];
					x /= primes[j];
				}
			}
		}

		// 计算组合数
		long long comb = 1;
		for (auto &it : primeCounts)
		{
			for (int i = 0; i < it.second; i++)
			{
				comb = comb * it.first % Mod;
			}
		}

		return ans * comb % Mod;
	}
};
```