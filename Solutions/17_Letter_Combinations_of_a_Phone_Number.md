# 递归回溯

```C++
void combineLetters(const std::string &digits, const std::vector<std::vector<char>> &letters, std::vector<std::string> &combs, std::string &comb, int index)
{
	if (index >= digits.size())
	{
		combs.push_back(comb);
		return;
	}
	for (auto &&c : letters[digits[index] - '2'])
	{
		comb += c;
		combineLetters(digits, letters, combs, comb, index + 1);
		comb.pop_back();
	}
}

class Solution
{
public:
	std::vector<std::string> letterCombinations(std::string digits)
	{
		if (digits.size() == 0)
			return {};

		const std::vector<std::vector<char>> letters = {{'a', 'b', 'c'}, {'d', 'e', 'f'}, {'g', 'h', 'i'}, {'j', 'k', 'l'}, {'m', 'n', 'o'}, {'p', 'q', 'r', 's'}, {'t', 'u', 'v'}, {'w', 'x', 'y', 'z'}};
		std::vector<std::string> combs;
		std::string comb;
		combineLetters(digits, letters, combs, comb, 0);
		return combs;
	}
};
```