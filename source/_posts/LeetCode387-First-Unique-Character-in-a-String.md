title: LeetCode387. First Unique Character in a String
date: 2017-02-19 20:54:24
tags:
- 数据结构
- 算法
---

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

Examples:
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```
Note: You may assume the string contain only lowercase letters.

```
class Solution {
public:
    int firstUniqChar(string s) {
        int result = -1, len = s.length();
        if (len == 0)
            return -1;
        if (len == 1)
            return 0;
        vector<int> count(26);
        for (int i = 0; i < len; i++) {
            count[s[i] - 'a']++;
        }
        
        for (int i = 0; i < len; i++) {
            if (count[s[i] - 'a'] == 1) {
                result = i;
                break;
            }
        }
        
        return result;
    }
};
```

