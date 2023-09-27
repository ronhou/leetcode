> [201. Bitwise AND of Numbers Range](https://leetcode.cn/problems/bitwise-and-of-numbers-range/)
> > [LeetCode官方题解](https://leetcode.cn/problems/bitwise-and-of-numbers-range/solutions/384938/shu-zi-fan-wei-an-wei-yu-by-leetcode-solution/)

# 位运算
根据LeetCode官方题解的思路，求解left和right的二进制公共前缀即可。

```C++
class Solution
{
public:
	int rangeBitwiseAnd(int left, int right)
	{
		int ans = 0;
		bool leftBit = false, rightBit = false;
		for (int i = sizeof(int) * 8 - 1; i >= 0; i--)
		{
			leftBit = !!(left & (1 << i));
			rightBit = !!(right & (1 << i));
			if (leftBit != rightBit)
				break;
			if (leftBit)
				ans |= (1 << i);
		}
		return ans;
	}
};
```
```C++
class Solution
{
public:
	int rangeBitwiseAnd(int left, int right)
	{
		int shift = 0;
		while (left < right)
		{
			left >>= 1;
			right >>= 1;
			++shift;
		}
		return left << shift;
	}
};
```
```C++ [Brian Kernighan Algorithm]
class Solution
{
public:
	int rangeBitwiseAnd(int left, int right)
	{
		while (left < right)
		{
			right &= (right - 1);
		}
		return right;
	}
};
```