title: LeetCode28. Implement strStr()
date: 2017-03-12 12:28:40
tags:
- 数据结构
- 算法
---

```
Implement strStr().

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

```

简单的做法双重循环，依次去比较，时间复杂度O(m*n)

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        int len1 = haystack.length(), len2 = needle.length();
        if (len2 > len1) return -1;
        if (len1 == 0 && len2 == 0) return 0;
        
        for (int i = 0; i < len1 - len2 + 1; i++) {
            int j = 0;
            for (; j < len2; j++) {
                if (haystack[i + j] != needle[j])
                    break;
            }
            if (j == len2)
                return i;
        }
        return -1;
    }
};
```