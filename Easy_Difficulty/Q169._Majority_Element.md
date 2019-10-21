### **投票算法**
#### [169. Majority Element](https://leetcode-cn.com/problems/majority-element/)
&emsp;&emsp;文首推荐LeetCode官方题解：[Q169. Majority Element - LeetCode官方题解 - 六种解法](https://leetcode-cn.com/problems/majority-element/solution/qiu-zhong-shu-by-leetcode-2/)  
&emsp;&emsp;首先，题目意思是查找数组里面的众数，且保证该众数出现次数大于`⌊ n/2 ⌋`。  
&emsp;&emsp;看到题目之后的第一反应，暴力出奇迹。使用一个hash_map<int, int>记录现有数字出现的次数，遍历更新出现的最大次数以及相应数字即可。

```
class Solution {
public:
	int majorityElement(vector<int>& nums) {
		int nMajorityElement;
		unsigned int nCount = 0;
		unordered_map<int, int> mapElementCount;
		for (vector<int>::size_type i = 0; i < nums.size(); ++i) {
			unordered_map<int, int>::iterator itFind = mapElementCount.find(nums[i]);
			if (itFind != mapElementCount.end()) {
				++itFind->second;
				if (itFind->second > nCount) {
					nCount = itFind->second;
					nMajorityElement = nums[i];
				}
			} else {
				mapElementCount.insert(std::make_pair(nums[i], 1));
				if (nCount < 1) {
					nCount = 1;
					nMajorityElement = nums[i];
				}
			}
		}

		return nMajorityElement;
	}
};
```
&emsp;&emsp;很高兴，提交之后就过了，毕竟是简单题，O(N)的时间复杂度和O(N)的空间复杂度，想必没有更好的时间复杂度的算法了，那么是不是可以优化空间复杂度呢？再次审题，题目中提到的，`保证数组中的众数出现次数大于⌊ n/2 ⌋`，这一点还没用到呢。  
&emsp;&emsp;实在没有想到优化的方法，于是看到了各位大神题解中的“投票算法”，叹为观止。

投票算法大致如下，详细证明等见文首推荐链接，主要是用到了众数出现次数大于`⌊ n/2 ⌋`这一点。
1. 用两个变量分别记录当前选择的MajorityElement，和其优胜的票数count；
2. 如果之前count等于0，则选择当前元素为MajorityElement；
3. 否则，如果当前元素也是MajorityElement，count自增1；
4. 否则，count自减1；
5. 重复第2~4步，最终留下的MajorityElement即最终解。

```
class Solution {
public:
	int majorityElement(vector<int>& nums) {
		int nMajorityElement, nCount = 0;
		for (vector<int>::size_type i = 0; i < nums.size(); ++i) {
			if (nCount <= 0) {
				nMajorityElement = nums[i];
				nCount = 1;
			} else if (nums[i] == nMajorityElement) {
				++nCount;
			} else {
				--nCount;
			}
		}

		return nMajorityElement;
	}
};
```

我果真不适合写题解，写文章啊！