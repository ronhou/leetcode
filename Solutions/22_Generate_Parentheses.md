# 递归回溯
+ 当左括号数量小于n时，可以考虑追加左括号
+ 当右括号数量小于左括号数量时，可以考虑追加右括号


```C++
class Solution
{
public:
	std::vector<std::string> generateParenthesis(int n)
	{
		std::vector<std::string> parentheses;
		std::string parenthesis;
		generateParentheses(n, 0, 0, parentheses, parenthesis);
		return parentheses;
	}

private:
	void generateParentheses(const int &n, int leftCount, int rightCount, std::vector<std::string> &parentheses, std::string &parenthesis)
	{
		if (leftCount >= n)
		{
			for (int i = rightCount; i < leftCount; i++)
				parenthesis += ')';
			parentheses.push_back(parenthesis);
			for (int i = rightCount; i < leftCount; i++)
				parenthesis.pop_back();
			return;
		}

		parenthesis += '(';
		generateParentheses(n, leftCount + 1, rightCount, parentheses, parenthesis);
		parenthesis.pop_back();
		if (rightCount < leftCount)
		{
			parenthesis.push_back(')');
			generateParentheses(n, leftCount, rightCount + 1, parentheses, parenthesis);
			parenthesis.pop_back();
		}
	}
};
```