# 二分查找
```C++ [线性扫描]
std::vector<std::vector<int>> insertIntoOrderedIntervals(std::vector<std::vector<int>> &intervals, std::vector<int> &newInterval)
{
	std::vector<std::vector<int>>::iterator itBegin = intervals.end(), itEnd = intervals.end();
	for (auto it = intervals.begin(); it != intervals.end(); ++it)
	{
		if (newInterval[0] <= it->at(1) && newInterval[1] >= it->at(0))
		{
			newInterval[0] = std::min(newInterval[0], it->at(0));
			newInterval[1] = std::max(newInterval[1], it->at(1));
			if (itBegin == intervals.end())
			{
				itBegin = it;
			}
			itEnd = it;
		}
	}
	if (itBegin == intervals.end())
	{
		auto it = std::lower_bound(intervals.begin(), intervals.end(), newInterval);
		intervals.insert(it, newInterval);
	}
	else
	{
		auto it = intervals.erase(itBegin, ++itEnd);
		it = intervals.insert(it, newInterval);
	}
	return intervals;
}
```
```C++ [二分查找]
void insertIntoOrderedIntervals(std::vector<std::vector<int>> &intervals, std::vector<int> &newInterval)
{
	auto it = std::lower_bound(intervals.begin(), intervals.end(), newInterval);
	if (it != intervals.begin())
	{
		const std::vector<int> &prevInterval = *(it - 1);
		if (prevInterval[1] >= newInterval[0])
		{
			--it;
		}
	}
	auto itBegin = it;
	while (it != intervals.end() && it->at(0) <= newInterval[1] && it->at(1) >= newInterval[0])
	{
		newInterval[0] = std::min(newInterval[0], it->at(0));
		newInterval[1] = std::max(newInterval[1], it->at(1));
		++it;
	}
	if (it != itBegin) {
		it = intervals.erase(itBegin, it);
	}
	it = intervals.insert(it, newInterval);
}
```