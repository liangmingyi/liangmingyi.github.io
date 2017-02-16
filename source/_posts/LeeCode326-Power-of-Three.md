title: LeeCode326. Power of Three
date: 2017-02-16 23:55:01
tags:
- 数据结构
- 算法
---

Given an integer, write a function to determine if it is a power of three.

Follow up:
Could you do it without using any loop / recursion?

#### 这个题的follow up是希望能不用循环或递归，一直想不出来解法，后来看到了下面的这种利用对数的换底公式来做的方法，也是拓宽了思路
log以3为底的n的对数等于以n为底10的对数除以3为底10的对数

```
class Solution {
public:
    bool isPowerOfThree(int n) {
        if (n <= 0)
            return false;
        return log10(n) / log10(3) - int(log10(n) / log10(3)) == 0;
    }
};
```
