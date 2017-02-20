title: LeeCode10. Regular Expression Matching
date: 2017-02-20 16:18:19
tags:
- 数据结构
- 算法
---

Implement regular expression matching with support for '.' and '*'.
```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

#### 如果p当前是\*号，则可以匹配一个或多个上一个字符，否则各自往后移动一个位置，然后再递归的去判断剩余的部分
#### 递归解法如下
```
class Solution {
public:
    bool isMatch(string s, string p) {
        int lens = s.length(), lenp = p.length();
        if (lenp == 0)
            return lens == 0;
            
        if (p[1] == '*') {
            int i = 0;
            while (i < lens && (s[i] == p[0] || p[0] == '.')) {
                if (isMatch(s.substr(i + 1), p.substr(2)))
                    return true;
                i++;
            }
            return isMatch(s, p.substr(2));
        } else {
            return lens > 0 && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p.substr(1));
        }
    }
};
```

#### 若S的前i个字符与P的前j个字符已经匹配，则只需考察剩余的字符即可，由于具有无后效性，故也可采用动态规划解法
```
dp[i][j] 表示s[0, i-1] 和 p[0, j-1]是否匹配
初始解:
    dp[0][0] = true; // 两个空字符串
    dp[0][1] 和 dp[1][0] 均是false
状态转义方程：
    如果p[j - 1]不是*也不是.则判断 s[i - 1] == p[j - 1] && dp[i - 1][j - 1]
    如果p[j - 1]是. 则dp[i][j] = dp[i - 1][j - 1]
    如果p[j - 1]是* 则分三种情况
        1. 匹配0个  dp[i][j] = dp[i][j - 2]
        2. 匹配1个  dp[i][j] = dp[i][j - 1]
        3. 匹配多个 dp[i][j] = dp[i - 1][j] && s[i - 1] == p[j - 2] || p[j - 2] == '.'
```

```
class Solution {
public:
    bool isMatch(string s, string p) {
        int lens = s.length(), lenp = p.length();
        vector<vector<bool>> dp(lens + 1, vector<bool>(lenp + 1, false));
        dp[0][0] = true;
        
        for (int i = 0; i < lens + 1; i++) {
            for (int j = 1; j < lenp + 1; j++) {
                if (p[j - 1] != '.' && p[j - 1] != '*') {
                    if (i > 0 && s[i - 1] == p[j - 1] && dp[i - 1][j - 1]) {
                        dp[i][j] = true;
                    }
                } else if (p[j - 1] == '.') {
                    if (i > 0 && dp[i - 1][j - 1]) {
                        dp[i][j] = true;
                    }
                } else if (j > 1) {
                    if (dp[i][j - 2] || dp[i][j - 1]) {
                        dp[i][j] = true;
                    } else if (i > 0 && dp[i - 1][j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.')) {
                        dp[i][j] = true;
                    }
                }
            }
        }
        
        for (int m = 1; m <= lens; m++)
            for (int n = 1; n <= lenp; n++)
                cout<<"m:"<<m<<" n:"<<n<<" dp:"<<dp[m][n]<<endl;
        
        return dp[lens][lenp];
    }
};
```
