> [190. Reverse Bits](https://leetcode.cn/problems/reverse-bits/)

## 模拟
+ 每次取原数字的最后一位追加到逆转数字的最后一位
+ 原数字向右移一位
+ 上述步骤执行32次

```C++
class Solution
{
public:
	uint32_t reverseBits(uint32_t n)
	{
		uint32_t ret = 0;
		for (int i = 0; i < 32; i++)
		{
			ret = (ret << 1) | (n & 1);
			n >>= 1;
		}
		return ret;
	}
};
```