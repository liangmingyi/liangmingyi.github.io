title: LeeCode345. Reverse Vowels of a String
date: 2017-02-17 11:29:38
tags:
- 数据结构
- 算法
---

Write a function that takes a string as input and reverse only the vowels of a string.

Example 1:
Given s = "hello", return "holle".

Example 2:
Given s = "leetcode", return "leotcede".

Note:
The vowels does not include the letter "y".

```
class Solution {
public:
    string reverseVowels(string s) {
        int len = s.length(), left = 0, right = len - 1;
        if (len < 2)
            return s;
        int dict[256] = {0};
        dict['a'] = 1; dict['A'] = 1;
        dict['e'] = 1; dict['E'] = 1;
        dict['i'] = 1; dict['I'] = 1;
        dict['o'] = 1; dict['O'] = 1;
        dict['u'] = 1; dict['U'] = 1;
        
        while (left < right) {
            while (left < len && dict[s[left]] == 0) { left++; };
            while (right > 0 && dict[s[right]] == 0) { right--; };
            
            if (left < right) {
                swap(s[left], s[right]);
                left ++; right --;
            }
        }
        
        return s;
    }
    
    void swap(char &a, char &b) {
        a = a ^ b;
        b = a ^ b;
        a = a ^ b;
    }
};
```
