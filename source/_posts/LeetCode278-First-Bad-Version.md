title: LeetCode278. First Bad Version
date: 2017-02-16 18:05:38
tags:
- 数据结构
- 算法
---

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.


#### 二分查找，需要特别注意的是 left + right 相加可能会超过 INT_MAX，所以需要转换成表示范围更大的数据类型，不然会有溢出的可能

```
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int left = 1, right = n, mid = (left + right) / 2;
        while (left <= right) {
            mid = (unsigned int)(left + right) / 2;
            if (isBadVersion(mid)) {
                if (mid == 1 || (mid > 1 && !isBadVersion(mid - 1))) {
                    break;
                } else {
                    right = mid - 1;
                }
            } else {
                left = mid + 1;
            }
        }
        
        return mid;
    }
};
```