# 栈
```C++
std::string simplifyAbsPathToCanonical(const std::string &path)
{
	std::stack<std::string> names;
	for (int i = 0, j = 0; i < path.size(); i = j)
	{
		// 跳过'/'
		while (i < path.size() && path[i] == '/')
		{
			++i;
		}

		// 获取文件（夹）名
		j = i;
		while (j < path.size() && path[j] != '/')
		{
			j++;
		}

		if (i < j)
		{
			std::string name = path.substr(i, j - i);
			if (name == "..")
			{
				if (!names.empty())
				{
					names.pop();
				}
			}
			else if (name != ".")
			{
				names.push(name);
			}
		}
	}

	std::string canonicalPath;
	while (!names.empty())
	{
		canonicalPath = '/' + names.top() + canonicalPath;
		names.pop();
	}
	return canonicalPath.empty() ? "/" : canonicalPath;
}
```