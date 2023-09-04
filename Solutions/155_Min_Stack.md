# 最小栈
```C++
class MinStack
{
public:
	MinStack()
	{
	}

	void push(int val)
	{
		m_stack.push(val);
		if (m_minStack.empty() || val < m_minStack.top())
		{
			m_minStack.push(val);
		}
		else
		{
			m_minStack.push(m_minStack.top());
		}
	}

	void pop()
	{
		m_stack.pop();
		m_minStack.pop();
	}

	int top()
	{
		return m_stack.top();
	}

	int getMin()
	{
		return m_minStack.top();
	}

private:
	std::stack<int> m_stack;
	std::stack<int> m_minStack;
};
```