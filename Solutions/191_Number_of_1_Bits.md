> [191. Number of 1 Bits](https://leetcode.cn/problems/number-of-1-bits/)

## 模拟
+ 每次取最后一位，如果最后一位是1，那么“汉明重量”加1
+ 数字向右移一位
+ 重复上面步骤，直至数字等于0

```C++
class Solution
{
public:
	int hammingWeight(uint32_t n)
	{
		int weight = 0;
		while (n > 0)
		{
			if (n & 1)
				++weight;
			n >>= 1;
		}
		return weight;
	}
};
```

## 位运算
+ 去掉最低位的1的简便方式是通过位运算：`x & (x - 1)`

```C++
class Solution
{
public:
	int hammingWeight(uint32_t n)
	{
		int weight = 0;
		while (n > 0)
		{
			++weight;
			n &= (n - 1);
		}
		return weight;
	}
};
```