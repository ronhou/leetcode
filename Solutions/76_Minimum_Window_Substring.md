# 滑动窗口
```C++ []
string minWindow(const string &source, const string &target)
{
	// 统计字符出现的频次
	std::unordered_set<char> charSet;
	std::unordered_map<char, int> charCounts;
	for (char c : target)
	{
		charSet.insert(c);
		++charCounts[c];
	}

	int minWindowSize = source.size() + 1, beginIndex = -1;
	for (int i = 0, l = -1; i < source.size(); i++)
	{
		// 窗口右侧右移
		char c = source[i];
		if (charCounts.find(c) != charCounts.end()) // targetCharSet.find(c) != targetCharSet.end()
		{
			// 初始化
			if (l < 0)
			{
				l = i;
			}

			// 字符频次减1
			--charCounts[c];
			// 字符频次足够了
			if (charCounts[c] == 0)
			{
				// 反向标记字符频次足够了
				charSet.erase(c);
				// 所有目标字符频次足够了
				if (charSet.empty())
				{
					if (i - l + 1 < minWindowSize)
					{
						// 更新最小窗口的信息
						minWindowSize = i - l + 1;
						beginIndex = l;
					}

					// 窗口左侧右移，缩小窗口，为窗口右侧右移准备
					++charCounts[source[l]];
					if (charCounts[source[l]] > 0)
					{
						charSet.insert(source[l]);
					}
					++l;
					while (l < i && (charCounts.find(source[l]) == charCounts.end() || charCounts[source[l]] < 0))
					{
						// 直接判断map值是否小于0，会默认填充0，需要先用find判断进行阻断
						if (charCounts.find(source[l]) != charCounts.end() && charCounts[source[l]] < 0)
						{
							++charCounts[source[l]];
						}
						++l;
					}
				}
			}
			else if (charCounts[c] < 0)
			{
				// 窗口左侧右移
				while (l < i && (charCounts.find(source[l]) == charCounts.end() || charCounts[source[l]] < 0))
				{
					// 直接判断map值是否小于0，会默认填充0，需要先用find判断进行阻断
					if (charCounts.find(source[l]) != charCounts.end() && charCounts[source[l]] < 0)
					{
						++charCounts[source[l]];
					}
					++l;
				}
			}
		}
	}

	return beginIndex < 0 ? "" : source.substr(beginIndex, minWindowSize);
}
```