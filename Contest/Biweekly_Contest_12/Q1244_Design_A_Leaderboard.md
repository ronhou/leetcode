```
bool func_ComprePlayer(const pair<int, int>& a, const pair<int, int>& b) {
	return a.second > b.second;
}

/**
 * 解法比较暴力：直接使用一个map来保存参赛者的分数
 * addScore：如果map中找到了该参赛者，则增加其分数；否则向map添加该参赛者及其分数
 * reset：如果map中找到了该参赛者，删除该参赛者
 * top：将map转成一个vector，并按照score排序，累加前K个的分数即可
 * 值得注意的是，题目中并没有提到分数相同时该怎么处理
 */
class Leaderboard {
public:
	Leaderboard() {
	}

	void addScore(int playerId, int score) {
		unordered_map<int, int>::iterator itFind = m_players.find(playerId);
		if (itFind != m_players.end())
			itFind->second += score;
		else
			m_players.insert(make_pair(playerId, score));
	}

	int top(int K) {
		vector<pair<int, int>> vecPlayers(m_players.begin(), m_players.end());
		sort(vecPlayers.begin(), vecPlayers.end(), func_ComprePlayer);

		int nSumScore = 0;
		for (vector<pair<int, int>>::size_type i = 0; i < K; ++i)
			nSumScore += vecPlayers[i].second;
		return nSumScore;
	}

	void reset(int playerId) {
		unordered_map<int, int>::iterator itFind = m_players.find(playerId);
		if (itFind != m_players.end())
			m_players.erase(itFind);
	}

private:
	unordered_map<int, int> m_players;
};

/**
 * Your Leaderboard object will be instantiated and called as such:
 * Leaderboard* obj = new Leaderboard();
 * obj->addScore(playerId,score);
 * int param_2 = obj->top(K);
 * obj->reset(playerId);
 */
```