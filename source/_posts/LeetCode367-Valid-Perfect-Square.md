title: LeetCode367. Valid Perfect Square
date: 2017-02-18 17:25:54
tags:
- 数据结构
- 算法
---

Given a positive integer num, write a function which returns True if num is a perfect square else False.

Note: Do not use any built-in library function such as sqrt.

Example 1:
```
Input: 16
Returns: True
```
Example 2:
```
Input: 14
Returns: False
```

```
class Solution {
public:
    bool isPerfectSquare(int num) {
        if (num < 1)
            return false;
        int tmp = mySqrt(num);
        return tmp * tmp == num;
    }
    
    int mySqrt(int num) {
        if (num < 2)
            return num;
        
        int l = 0, r = num / 2 + 1, mid = (l + r) / 2, tmp;
        while (l <= r) {
            mid = (l + r) / 2;
            tmp = num / mid;
            
            if (tmp > mid) {
                l = mid + 1;
            } else if (tmp < mid) {
                r = mid - 1;
            } else {
                break;
            }
        }
        
        return mid;
    }
};
```

