title: LeetCode728. Self Dividing Numbers
date: 2017-11-24 00:12:22
tags:
- 数据结构
- 算法
---

A self-dividing number is a number that is divisible by every digit it contains.

For example, 128 is a self-dividing number because 128 % 1 == 0, 128 % 2 == 0, and 128 % 8 == 0.

Also, a self-dividing number is not allowed to contain the digit zero.

Given a lower and upper number bound, output a list of every possible self dividing number, including the bounds if possible.

```
Example 1:
Input: 
left = 1, right = 22
Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]
```

Note:
The boundaries of each input argument are 1 <= left <= right <= 10000.

依次对范围内的每个数做判断，比较简单

```
class Solution {
public:
    bool isSelfDivid(int num) {
        int t1 = num, t2;
        bool ret = true;
        
        while (t1 > 0) {
            t2 = t1 % 10;
            if (t2 == 0) {
                ret = false;
                break;
            }
            
            if (num % t2 != 0) {
                ret = false;
                break;
            }
            t1 = t1 / 10;
        }
        return ret;
    }
    
    vector<int> selfDividingNumbers(int left, int right) {
        vector<int> ret;
        for (; left <= right; left ++) {
            if (isSelfDivid(left)) {
                ret.push_back(left);
            }
        }
        return ret;
    }
};
```