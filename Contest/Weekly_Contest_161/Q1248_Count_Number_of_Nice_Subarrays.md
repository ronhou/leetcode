```
class Solution {
public:
	int numberOfSubarrays(vector<int>& nums, int k) {
		vector<int> evens;
		int nLastOddPos = -1;
		for (size_t i = 0; i < nums.size(); ++ i) {
			if (nums[i] % 2 != 0) {
				evens.push_back(i - nLastOddPos - 1);
				nLastOddPos = i;
			}
		}
		evens.push_back(nums.size() - nLastOddPos - 1);

		size_t nCount = 0;
		for (size_t i = k; i < evens.size(); ++i) {
			nCount += (evens[i] + 1) * (evens[i - k] + 1);
		}

		return nCount;
	}
};
```