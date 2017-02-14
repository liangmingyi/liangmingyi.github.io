title: LeeCode125. Valid Palindrome
date: 2017-02-15 00:05:29
tags:
- 数据结构
- 算法
---

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.

Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.


```
class Solution {
public:
    bool isPalindrome(string s) {
        bool result = true;
        int len = s.length(), idx = 0;
        
        if (len <= 1)
            return true;
        
        char c[len];
        
        for (int i = 0; i < len; i++) {
            if ((s[i] >= 'a' && s[i] <= 'z') || (s[i] >= 'A' && s[i] <= 'Z') || (s[i] >= '0' && s[i] <= '9')) {
                c[idx++] = s[i];
            }
        }

        int left = 0, right = idx - 1;
        while (left < right) {
            if (!(c[left] - c[right] == 0 || (c[left] >= 'A' && c[left] <= 'z' && c[right] >= 'A' && c[right] <= 'z' && abs(c[left] - c[right]) == 'a' - 'A'))) {
                result = false;
                break;
            } else {
                left++;
                right--;
            }
        }
        
        return result;
    }
};
```