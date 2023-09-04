# 排序+贪心
```C++
int getMinArrowShots(std::vector<std::vector<int>> &points)
{
	std::sort(points.begin(), points.end(),
			  [](const std::vector<int> &a, const std::vector<int> &b) -> bool
			  {
				  return a[1] < b[1];
			  });

	int pos = points[0][1], count = 1;
	for (int i = 0; i < points.size(); i++)
	{
		if (points[i][0] > pos)
		{
			pos = points[i][1];
			++count;
		}
	}
	return count;
}
```