> [67. Add Binary](https://leetcode.cn/problems/add-binary/)
## 模拟
使用字符串模拟二进制加法

```C++
class Solution
{
public:
	std::string addBinary(std::string a, std::string b)
	{
		std::string sumStr;
		int sum = 0, minLen = std::min(a.size(), b.size());
		for (int i = 1; i <= minLen; i++)
		{
			sum += a[a.size() - i] - '0' + b[b.size() - i] - '0';
			sumStr.push_back('0' + (sum & 1));
			sum >>= 1;
		}

		const string &s = a.size() > b.size() ? a : b;
		for (int i = minLen + 1; i <= s.size(); i++)
		{
			sum += s[s.size() - i] - '0';
			sumStr.push_back('0' + (sum & 1));
			sum >>= 1;
		}

		if (sum > 0)
			sumStr.push_back('1');

		std::reverse(sumStr.begin(), sumStr.end());
		return sumStr;
	}
};
```