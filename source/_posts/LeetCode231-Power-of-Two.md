title: LeetCode231. Power of Two
date: 2017-02-15 17:42:12
tags:
- 数据结构
- 算法
---

Given an integer, write a function to determine if it is a power of two.


```
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if (n <= 0)
            return false;
        return !(n & (n - 1));
    }
};
```