# 模拟
```C++
int maxCapturedForts(vector<int> &forts)
{
	int maxCnt = 0, idx = -1;
	for (int i = 0; i < forts.size(); i++)
	{
		if (forts[i])
		{
			if (idx >=0 && forts[i] + forts[idx] == 0)
				maxCnt = std::max(maxCnt, i - idx - 1);
			idx = i;
		}
	}
	return maxCnt;
}
```