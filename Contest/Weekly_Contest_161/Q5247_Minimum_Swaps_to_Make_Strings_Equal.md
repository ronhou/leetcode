刚开始看到题目时并没有什么思路，看着打着简单题标签的这道题，于是我打算暴力地解法试一试。  
首先特殊处理一下集中情况，比如两个串长度不一样的时候，又比如x和y的个数不是偶数的时候，这两种情况直接返回-1。  
接下来就是真正地解题思路了：
1. 首先，如果某个位置`i`，`s1[i]==s2[i]`，那么这个位置不会用来交换，直接忽略跳过；
2. 然后，如果`s1[i]!=s2[i]`时，向后找一个位置`j`，`s1[j]!=s2[j]`，用来交换`s1[i]`和`s2[j]`；如题目给出的样例所示，主要有以下两种情况：
   1. 如果`s1[j]!=s1[i]`，比如`"x***y"`和`"y**x"`，那么刚好，只用交换一次就能解决；
   2. 如果`s1[j]==s1[i]`，比如`"x***x"`和`"y**y"`，那么就得交换两次才行。
3. 因此，我们优先寻找2.1这种情况，如果没找到的话，然后再找2.2这种情况。毕竟优先交换次数少的处理。

下面就是相应代码，提交之后发现过了，开心。果然遇到简单题的时候不要想太多，时间复杂度我都没管！
```
class Solution {
public:
	int minimumSwap(string s1, string s2) {
		if (s1.size() != s2.size())
			return -1;

		int nCountX = 0, nCountY = 0;
		for (string::size_type i = 0; i < s1.size(); ++i) {
			if (s1[i] == 'x')
				++nCountX;
			else if (s1[i] == 'y')
				++nCountY;

			if (s2[i] == 'x')
				++nCountX;
			else if (s2[i] == 'y')
				++nCountY;
		}

		if ((nCountX % 2 != 0) || (nCountY % 2 != 0))
			return -1;

		int nSwapCount = 0;
		for (string::size_type i = 0; i < s1.size(); ++i) {
			if (s1[i] == s2[i])
				continue;

			string::size_type j = i + 1;
			for (; j < s1.size(); ++j) {
				if (s1[j] == s1[i] && s1[j] != s2[j]) {
					s1[i] = s2[i];
					s2[j] = s1[j];
					++nSwapCount;
					break;
				}
			}

			if (j < s1.size())
				continue;

			for (j = i + 1; j < s1.size(); ++j) {
				if (s1[j] != s1[i] && s1[j] != s2[j]) {
					s1[i] = s2[i];
					s1[j] = s2[j];
					nSwapCount += 2;
					break;
				}
			}

			if (j >= s1.size())
				return -1;
		}

		return nSwapCount;
	}
};
```