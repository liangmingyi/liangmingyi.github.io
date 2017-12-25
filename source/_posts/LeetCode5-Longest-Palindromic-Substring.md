title: LeetCode5. Longest Palindromic Substring
date: 2017-12-25 23:21:46
tags:
- 数据结构
- 算法
---

[问题来自LeetCode5](https://leetcode.com/problems/longest-palindromic-substring/description/)

用dp来解决比较好理解
状态转移方程dp[j][i]表示以j为起点,i为终点的子串是否为回文串
dp[j][i] = (s[j] == s[i] && s[j + 1][i - 1])
初值
当j == i时,dp[j][i] = true
当i - j = 1时,s[i] == s[j]则dp[j][i] = true

```
class Solution {
public:
    string longestPalindrome(string s) {
        int len = s.length();
        if (len < 2)
            return s;
        
        bool dp[len][len] = {false};
        string ret = "";
        int maxLen = 0;
        
        for (int i = 0; i < len; i++) { // i是终点
            int j = i; // j为起点
            while (j >= 0) {
                if (s[j] == s[i] && (i - j < 2 || dp[j + 1][i - 1])) {
                    dp[j][i] = true;
                    if (i - j + 1 > maxLen) {
                        maxLen = i - j + 1;
                        ret = s.substr(j, maxLen);
                    }
                }
                j --;
            }
        }
        return ret;
    }
};
```