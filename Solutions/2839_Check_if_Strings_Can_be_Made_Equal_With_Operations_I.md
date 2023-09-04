> 第 112 场双周赛，第1题（#2839）：  
> [2839. Check if Strings Can be Made Equal With Operations I](https://leetcode.cn/contest/biweekly-contest-112/problems/check-if-strings-can-be-made-equal-with-operations-i/)

# 暴力枚举
```C++
class Solution
{
public:
	bool canBeEqual(string s1, string s2)
	{
		if (s1 == s2)
			return true;

		string s3 = s1;
		std::swap(s3[0], s3[2]);
		if (s3 == s2)
			return true;

		std::swap(s3[1], s3[3]);
		if (s3 == s2)
			return true;

		std::swap(s3[0], s3[2]);
		return s3 == s2;
	}
};
```