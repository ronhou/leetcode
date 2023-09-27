> [137. Single Number II](https://leetcode.cn/problems/single-number-ii/)

## 哈希表
使用哈希表统计各个数字出现的频次

```C++ [哈希表]
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        std::unordered_map<int, unsigned short> numMap;
        for (const int& num : nums) {
            numMap[num] = numMap[num] + 1;
        }
        for (auto& it : numMap) {
            if (it.second == 1) {
                return it.first;
            }
        }
        return 0;
    }
};
```

## 位运算
[官方题解：统计每个bit位的合](https://leetcode.cn/problems/single-number-ii/solutions/746993/zhi-chu-xian-yi-ci-de-shu-zi-ii-by-leetc-23t6/)