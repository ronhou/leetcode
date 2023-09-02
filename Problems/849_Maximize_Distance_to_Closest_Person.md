# 线性扫描进行模拟
```C++ []
int maxDistanceToClosest(vector<int> &seats)
{
	int personIndex = -1, maxDist = 0;
	for (int i = 0; i < seats.size(); i++)
	{
		if (seats[i])
		{
			personIndex = i;
		}
		else
		{
			if (personIndex == -1 || i == seats.size() - 1)
			{
				maxDist = std::max(maxDist, i - personIndex);
			}
			else
			{
				maxDist = std::max(maxDist, (i - personIndex + 1) / 2);
			}
		}
	}
	return maxDist;
}
```