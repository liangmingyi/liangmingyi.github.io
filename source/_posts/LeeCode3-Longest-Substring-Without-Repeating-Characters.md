title: LeeCode3. Longest Substring Without Repeating Characters
date: 2017-02-14 01:26:00
tags: 数据结构, 算法
---

Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.



### 扫一遍字符串即可得到需要的结果 时间复杂度O(N)
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int a[256] = {-1}, len = s.length(), result = 0, maxlen = 0, j, start = 0;
        for (j = 0; j < len; j++) {
            int cur = s[j];
            if (a[cur] > start)
                start = a[cur];
            result = j - start;
            maxlen = max(result, maxlen);
            a[cur] = j;
        }
        return maxlen;
    }
};
```
