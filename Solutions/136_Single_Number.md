> [136. Single Number](https://leetcode.cn/problems/single-number/)

## 位运算
基于异或运算的性质：`x xor x = 0`，因此将题目中数组中的数字全都亦或在一起即可得到题目答案。

```C++
class Solution
{
public:
	int singleNumber(std::vector<int> &nums)
	{
		int x = 0;
		for (auto &&num : nums)
		{
			x ^= num;
		}
		return x;
	}
};
```