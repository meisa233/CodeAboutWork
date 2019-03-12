### 题目描述：
```
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);
Example 1:

Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
Example 2:

Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

### 答案：
```C++
class Solution {
public:
	string convert(string s, int numRows) {
		if (numRows == 1) return s;
		if (numRows == 2) {
			string a = "";
			string b = "";
			for (int i = 0; i<s.size(); i++) {
				if (i % 2 == 0)a = a + s[i];
				else b = b + s[i];
			}
			return a + b;
		}
		string* result = new string[numRows];
		for (int i = 0; i<numRows; i++)
			result[i] = "";
		int LoopNum1 = numRows;
		int LoopNum2 = numRows + numRows -2;
		for (int i = 0; i < s.size(); i++) {
			if (i%LoopNum2 < numRows) {
				result[i%LoopNum2] = result[i%LoopNum2] + s[i];
			}
			else {
				result[numRows - 1 - (i%LoopNum2 - (numRows - 1))] = result[numRows - 1 - (i%LoopNum2 - (numRows - 1))] + s[i];
			}
		}
		string Result_ = "";
		for (int i = 0; i< numRows; i++) {
			Result_ = Result_ + result[i];
		}
		return Result_;
	}
};
```
别人的答案
```C++
string convert(string s, int nRows) {
    
    if (nRows <= 1)
        return s;

    const int len = (int)s.length();
    string *str = new string[nRows];

    int row = 0, step = 1;
    for (int i = 0; i < len; ++i)
    {
        str[row].push_back(s[i]);

        if (row == 0)
            step = 1;
        else if (row == nRows - 1)
            step = -1;

        row += step;
    }

    s.clear();
    for (int j = 0; j < nRows; ++j)
    {
        s.append(str[j]);
    }

    delete[] str;
    return s;
}
```
