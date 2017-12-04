title: LeetCode72. Edit Distance
date: 2017-12-04 21:00:56
tags:
- 数据结构
- 算法
---

Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

a) Insert a character
b) Delete a character
c) Replace a character


最短编辑距离，比较经典的DP求解问题

状态转移方程
dp[i][j] = min(dp[i - 1][j - 1] + same(word1[i - 1], word2[j - 1]), dp[i - 1][j] + 1, dp[i][j - 1] + 1)

初值
dp[0][j] = j
dp[i][0] = i

```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.length(), n = word2.length();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                if (i == 0) {
                    dp[i][j] = j;
                } else if (j == 0) {
                    dp[i][j] = i;
                } else {
                    dp[i][j] = min(dp[i - 1][j - 1] + (word1[i - 1] == word2[j - 1] ? 0 : 1), 
                                   min(dp[i - 1][j] + 1, dp[i][j - 1] + 1)
                                  );   
                }
            }
        }
        return dp[m][n];
    }
};
```