```
class Solution {
public:
	string encode(int num) {
		unsigned int nBitCount = 0, nPower = 1, numX = num;
		while (numX >= nPower) {
			numX -= nPower;
			++nBitCount;
			nPower <<= 1;
		}

		nPower >>= 1;
		string strEncode = "";
		while (nBitCount > 0) {
			if (numX >= nPower) {
				strEncode.push_back('1');
				numX -= nPower;
			} else {
				strEncode.push_back('0');
			}
			--nBitCount;
			nPower >>= 1;
		}

		return strEncode;
	}
};
```