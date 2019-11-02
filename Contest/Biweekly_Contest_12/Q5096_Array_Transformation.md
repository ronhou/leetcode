```
class Solution {
public:
	/**
	 * 还是直接暴力的解法
	 * 1. 用一个bool变量来标记数组是否有被转换，初始化为true
	 * 2. 用一个新的数组来存放转换后的数组，转换结束后，重置回原来的数组arr
	 */
	vector<int> transformArray(vector<int>& arr) {
		vector<int>::size_type nSize = arr.size();
		vector<int> newArr(nSize);
		bool bChanged = true;
		while (bChanged) {
			bChanged = false;
			for (vector<int>::size_type i = 1; i < nSize - 1; ++i) {
				if (arr[i] < arr[i - 1] && arr[i] < arr[i + 1]) {
					bChanged = true;
					newArr[i] = arr[i] + 1;
				}
				else if (arr[i] > arr[i - 1] && arr[i] > arr[i + 1]) {
					bChanged = true;
					newArr[i] = arr[i] - 1;
				} else {
					newArr[i] = arr[i];
				}
			}

			if (bChanged) {
				for (vector<int>::size_type i = 1; i < nSize - 1; ++i)
					arr[i] = newArr[i];
			}
		}

		return arr;
	}
};
```