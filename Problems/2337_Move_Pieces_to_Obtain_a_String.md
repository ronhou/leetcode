# 双指针
```C++ []
bool canChange(string source, string target)
{
	int j = 0;
	for (int i = 0; i < target.size(); i++)
	{
		if (target[i] == '_')
		{
			continue;
		}
		while (j < source.size() && source[j] == '_')
		{
			j++;
		}
		if (j >= source.size() || source[j] != target[i])
		{
			return false;
		}
		if (target[i] == 'L' && j < i || target[i] == 'R' && j > i)
		{
			return false;
		}
		j++;
	}
	for (; j < source.size(); j++)
	{
		if (source[j] != '_')
		{
			return false;
		}
	}
	return true;
}
```