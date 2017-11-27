title: LeetCode263. Ugly Number
date: 2017-02-15 23:10:58
tags:
- 数据结构
- 算法
---

Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.

Note that 1 is typically treated as an ugly number.

```
class Solution {
public:
    bool isUgly(int num) {
        if (num == 0)
            return false;
        
        while (num % 2 == 0) {
            num /= 2;
        }
        
        while (num % 3 == 0) {
            num /= 3;
        } 
        
        while (num % 5 == 0) {
            num /= 5;
        }
        
        return num == 1;
    }
};
```
