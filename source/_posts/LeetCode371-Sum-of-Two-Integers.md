title: LeetCode371. Sum of Two Integers
date: 2017-02-18 17:52:09
tags:
- 数据结构
- 算法
---

Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

Example:
Given a = 1 and b = 2, return 3.

#### 在不考虑进位的情况下 a ^ b，只考虑进位的情况下 (a & b) << 1，这两个结果相加为和的结果

```
class Solution {
public:
    int getSum(int a, int b) {
        int sum = a ^ b;
        int extra = (a & b) << 1;
        
        if (extra != 0)
            return getSum(sum, extra);
        
        return sum;
    }
};
```
