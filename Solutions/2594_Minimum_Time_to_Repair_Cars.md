# 二分查找
> 官方题解：[修车的最少时间](https://leetcode.cn/problems/minimum-time-to-repair-cars/solutions/2425409/xiu-che-de-zui-shao-shi-jian-by-leetcode-if20/)

```C++
bool canRepairAllCarsInTime(const std::vector<int> &ranks, int cars, long long time) {
	int count = 0;
	for (auto &&rank : ranks)
	{
		count += std::sqrt(time / rank);
		if (count >= cars)
			return true;
	}
	return false;
}

long long minTimeToRepairCars(std::vector<int> &ranks, int cars)
{
	std::sort(ranks.begin(), ranks.end());
	long long l = 0, r = (long long)ranks[0] * cars * cars, mid = (l + r) / 2;
	while (l < r)
	{
		mid = (l + r) / 2;
		if (canRepairAllCarsInTime(ranks, cars, mid)) {
			r = mid;
		} else {
			l = mid + 1;
		}
	}
	return l;
}
```