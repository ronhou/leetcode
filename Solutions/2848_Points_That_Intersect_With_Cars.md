# 排序
将nums按照从小到大进行排序，线性扫描计算即可。
+ 记录全局的前面车辆的终点END
+ 如果start<sub>i</sub>大于END，则第i辆车与前面的车不相交
  + 将整辆车的长度加入到总长度
+ 否则，如果end<sub>i</sub>大于END，则第i辆车与前面的车相交，且有超过部分
  + 将超过部分进行累加到总长度

```C++
class Solution
{
public:
	int numberOfPoints(vector<vector<int>> &nums)
	{
		std::sort(nums.begin(), nums.end());

		int totalCount = 0, end = -1;
		for (auto &&num : nums)
		{
			if (num[0] > end)
				totalCount += num[1] - num[0] + 1;
			else if (num[1] > end)
				totalCount += num[1] - end;
			end = std::max(end, num[1]);
		}
		return totalCount;
	}
};
```