# 栈
```C++
int evaluateReversePolishNotation(std::vector<std::string> &tokens)
{
	std::stack<int> nums;
	for (const std::string &token : tokens)
	{
		if (token == "+" || token == "-" || token == "*" || token == "/")
		{
			int num1 = nums.top();
			nums.pop();
			int num2 = nums.top();
			nums.pop();
			if (token == "+")
				nums.push(num1 + num2);
			else if (token == "-")
				nums.push(num2 - num1);
			else if (token == "*")
				nums.push(num1 * num2);
			else if (token == "/")
				nums.push(num2 / num1);
		}
		else
		{
			nums.push(std::stoi(token));
		}
	}

	int ans = nums.top();
	nums.pop();
	return ans;
}
```