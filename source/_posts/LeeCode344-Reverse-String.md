title: LeeCode344. Reverse String
date: 2017-02-17 00:18:15
tags:
- 数据结构
- 算法
---

Write a function that takes a string as input and returns the string reversed.

Example:
Given s = "hello", return "olleh".

```
class Solution {
public:
    string reverseString(string s) {
        int l = s.length(), left = 0, right = l - 1;
        if (l < 2)
            return s;
        while (left < right) {
            swap(s[left++], s[right--]);
        }
        return s;
    }
    
    void swap(char &a, char &b) {
        char tmp = a;
        a = b;
        b = tmp;
    }
};
```