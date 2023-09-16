# 递归回溯

```C++
class Solution
{
public:
	std::vector<std::vector<int>> combine(int n, int k)
	{
		m_combinations.clear();
		m_curCombination.clear();
		combineKFromN(n, k);
		return m_combinations;
	}

private:
	void combineKFromN(int n, int k)
	{
		if (m_curCombination.size() >= k)
		{
			m_combinations.push_back(m_curCombination);
			return;
		}
		for (int i = m_curCombination.empty() ? 1 : m_curCombination.back() + 1; i <= n - k + m_curCombination.size() + 1; i++)
		{
			m_curCombination.push_back(i);
			combineKFromN(n, k);
			m_curCombination.pop_back();
		}
	}

private:
	std::vector<std::vector<int>> m_combinations;
	std::vector<int> m_curCombination;
};
```