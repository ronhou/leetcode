# 栈
经典题目：使用后进后出特性的栈处理表达式求值
```C++
typedef int (*OpFunc)(int, int);

// 定义运算符，包括其表达式，优先级，以及执行函数
struct Operator
{
	std::string expr;
	int priority;
	OpFunc exec;

	Operator()
	{
		priority = -1;
	}

	Operator(const std::string &exp, int pri, OpFunc op)
	{
		expr = expr;
		priority = pri;
		exec = op;
	}
};

class Calculator
{
private:
	enum ExprType
	{
		ET_None = 0,
		ET_LeftBracket = 1,
		ET_RightBracket = 2,
		ET_Number = 3,
		ET_Operator = 4,
	};

public:
	int calculate(const std::string &s)
	{
		std::stack<int> operands;
		std::stack<std::string> operators;
		int index = 0;
		std::string word;
		ExprType lastType = ET_None;
		while ((word = peekNextWord(s, index)) != "")
		{
			if (isBracket(word, true))
			{
				operators.push(word);
				lastType = ET_LeftBracket;
			}
			else if (isBracket(word, false))
			{
				while (!operators.empty() && !isBracket(operators.top(), true))
				{
					perform(operands, operators);
				}
				if (!operators.empty() && isBracket(operators.top(), true))
				{
					operators.pop();
				}
				lastType = ET_RightBracket;
			}
			else if (isNum(word))
			{
				operands.push(std::stoi(word));
				lastType = ET_Number;
			}
			else if (isOperator(word))
			{
				// 增加ExprType，为了特殊处理-2这种负数情况
				if (lastType == ET_None || lastType == ET_LeftBracket)
				{
					operands.push(0);
				}

				while (!operators.empty() && getPriority(word) <= getPriority(operators.top()))
				{
					perform(operands, operators);
				}
				operators.push(word);
				lastType = ET_Operator;
			}
		}

		while (!operators.empty())
		{
			perform(operands, operators);
		}
		return operands.top();
	}

	std::string peekNextWord(const std::string &s, int &index)
	{
		while (index < s.size() && s[index] == ' ')
			++index;
		if (index >= s.size())
			return "";
		int startIndex = index;
		if (isDigit(s[index]))
		{
			while (index < s.size() && isDigit(s[index]))
				++index;
		}
		else
		{
			// Todo：处理长度大于1的运算符，拆分连续运算符（比如"*("）等
			++index;
		}
		return s.substr(startIndex, index - startIndex);
	}

	void perform(std::stack<int> &operands, std::stack<std::string> &operators)
	{
		std::string op = operators.top();
		operators.pop();
		int num2 = operands.top();
		operands.pop();
		int num1 = !operands.empty() ? operands.top() : 0;
		operands.pop();
		OpFunc exec = getOpFunc(op);
		int num0 = exec(num1, num2);
		operands.push(num0);
	}

	bool isDigit(char c)
	{
		return '0' <= c && c <= '9';
	}

	// 简单判断是否是数字
	bool isNum(const std::string &word)
	{
		return isDigit(word[0]);
	}

	bool isBracket(const std::string &op, bool isLeft)
	{
		return isLeft ? op == "(" : op == ")";
	}

	bool isOperator(const std::string &exp)
	{
		return Operators.find(exp) != Operators.end();
	}

	int getPriority(const std::string &op)
	{
		return Operators[op].priority;
	}

	OpFunc getOpFunc(const std::string &op)
	{
		return Operators[op].exec;
	}

private:
	static std::unordered_map<std::string, Operator> Operators;
};

std::unordered_map<std::string, Operator> Calculator::Operators = {
	{"+", {"+", 1, [](int a, int b) -> int
		   { return a + b; }}},
	{"-", {"-", 1, [](int a, int b) -> int
		   { return a - b; }}},
	{"*", {"*", 2, [](int a, int b) -> int
		   { return a * b; }}},
	{"/", {"/", 2, [](int a, int b) -> int
		   { return a / b; }}},
	{"%", {"%", 2, [](int a, int b) -> int
		   { return a % b; }}},
};
```