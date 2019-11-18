1. 先将regions转换成一棵树
2. 然后沿着region1一直向根走，同时进行标记该区域region已被访问过；
3. 再沿着region2一直向根走，遇到了某个被访问过的region即为答案。
```
class Solution {
public:
	string findSmallestRegion(vector<vector<string>>& regions, string region1, string region2) {
		unordered_map<string, string> regParentMap;
		for (size_t i = 0; i < regions.size(); ++i) {
			for (size_t j = 1; j < regions[i].size(); ++j) {
				regParentMap.insert(make_pair(regions[i][j], regions[i][0]));
			}
		}

		unordered_set<string> regVisitedFlag;
		regVisitedFlag.insert(region1);

		unordered_map<string, string>::iterator itFind = regParentMap.find(region1);
		while (itFind != regParentMap.end()) {
			region1 = itFind->second;
			regVisitedFlag.insert(region1);
			itFind = regParentMap.find(region1);
		}

		unordered_set<string>::iterator itVisited = regVisitedFlag.find(region2);
		while (itVisited == regVisitedFlag.end()) {
			itFind = regParentMap.find(region2);
			region2 = itFind->second;
			itVisited = regVisitedFlag.find(region2);
		}

		return region2;
	}
};
```