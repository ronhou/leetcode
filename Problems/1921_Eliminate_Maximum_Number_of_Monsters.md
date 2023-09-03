# 排序后贪心
+ 计算怪物到达城市的时间：`cost = ceil(dist/speed)`
+ 按照时间从小到大进行排序
+ 从1-index开始，怪物能被消灭需满足：`index < costs[index]`（第0个怪物必定能被消灭）
```C++
int countEliminateMonsters(std::vector<int> &dist, std::vector<int> &speed)
{
	std::vector<int> costs(dist.size(), 0);
	for (int i = 0; i < dist.size(); i++)
		costs[i] = dist[i] / speed[i] + (dist[i] % speed[i] != 0 ? 1 : 0);
	std::sort(costs.begin(), costs.end());

	int index = 1;
	while (index < costs.size(); && index < costs[index])
		++index;
	return index;
}
```