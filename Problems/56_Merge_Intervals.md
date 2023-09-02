# 排序
```C++ [排序]
std::vector<std::vector<int>> mergeIntervals(std::vector<std::vector<int>> &intervals)
{
	std::vector<std::vector<int>> mergedIntervals;
	std::sort(intervals.begin(), intervals.end(), [](const std::vector<int> &a, std::vector<int> &b) -> bool
			  { return a[0] != b[0] ? a[0] < b[0] : a[1] < b[1]; });
	std::vector<int> mergedInterval;
	for (int i = 0; i < intervals.size(); i++)
	{
		if (mergedInterval.empty())
		{
			mergedInterval = intervals[i];
		}
		else if (intervals[i][0] <= mergedInterval[1])
		{
			mergedInterval[1] = std::max(mergedInterval[1], intervals[i][1]);
		}
		else
		{
			mergedIntervals.push_back(mergedInterval);
			mergedInterval = intervals[i];
		}
	}
	mergedIntervals.push_back(mergedInterval);
	return mergedIntervals;
}
```
```C++ [排序]
std::vector<std::vector<int>> mergeIntervals(std::vector<std::vector<int>> &intervals)
{
	std::vector<std::vector<int>> mergedIntervals;
	std::sort(intervals.begin(), intervals.end());
	for (const std::vector<int> &interval : intervals)
	{
		if (mergedIntervals.empty() || interval[0] > mergedIntervals.back()[1])
		{
			mergedIntervals.push_back(interval);
		}
		else
		{
			mergedIntervals.back()[1] = std::max(mergedIntervals.back()[1], interval[1]);
		}
	}
	return mergedIntervals;
}
```