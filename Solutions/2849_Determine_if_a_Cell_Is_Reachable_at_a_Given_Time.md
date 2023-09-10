# 数学模拟
+ 特殊情况：起点即终点，且t等于1
+ 从`(sx, sy)`到`(fx, fy)`所需的最小步数为：`max(abs(fx - sx), abs(fy - sy))`
+ t大于等于最小步数即可到达目标

```C++
class Solution
{
public:
	bool isReachableAtTime(int sx, int sy, int fx, int fy, int t)
	{
		if (sx == fx && sy == fy)
			return t != 1;

		int xSteps = std::abs(fx - sx);
		int ySteps = std::abs(fy - sy);
		int minSteps = std::max(xSteps, ySteps);
		return minSteps <= t;
	}
};
```