# 枚举
按照示例解释的方式进行枚举即可
```C++
long long waysToBuy(int total, int cost1, int cost2)
{
	long long ways = 0;
	int maxCost = std::max(cost1, cost2), minCost = std::min(cost1, cost2);
	for (int cnt1 = 0; cnt1 <= total; cnt1 += maxCost)
	{
		int rest = total - cnt1;
		int cnt2 = rest / minCost;
		ways = ways + cnt2 + 1;
	}
	return ways;
}
```