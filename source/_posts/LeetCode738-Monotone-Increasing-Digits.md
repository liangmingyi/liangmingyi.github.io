title: LeetCode738. Monotone Increasing Digits
date: 2017-12-03 18:52:14
tags:
- 数据结构
- 算法
---

Given a non-negative integer N, find the largest number that is less than or equal to N with monotone increasing digits.

(Recall that an integer has monotone increasing digits if and only if each pair of adjacent digits x and y satisfy x <= y.)

Example 1:

```
Input: N = 10
Output: 9
```

Example 2:

```
Input: N = 1234
Output: 1234
```

Example 3:

```
Input: N = 332
Output: 299
```

Note: N is an integer in the range [0, 10^9].

比较笨的办法，从N开始，一个一个的判断，直到找到符合要求的数。但其中有很多判断是多余的，可以做一些优化，减少判断的次数。比如当检测332不符合要求，则331、330都是没必要去判断的。


```
class Solution {
public:
    
    bool checkNum(int &N) {
        int t = N, t1 = 9, t2, offset = 0;
        while (t) {
            t2 = t % 10;
            if (t1 < t2) {
                N = t * pow(10, offset);
                return false;
            }
            t1 = t2;
            t = t / 10;
            offset ++;
        }   
        return true;
    }
    
    int monotoneIncreasingDigits(int N) {
        int ret = N;
        while (ret-- > 0) {
            if (checkNum(ret))
                return ret;
        }
        return 0;
    }
};
```