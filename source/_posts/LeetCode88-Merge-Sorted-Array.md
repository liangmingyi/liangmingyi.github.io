title: LeetCode88. Merge Sorted Array
date: 2017-02-14 19:48:50
tags: 
- 数据结构
- 算法
---

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.


从后往前归并

```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        if (m == 0 && n == 0)
            return ;
        if (n == 0)
            return ;
        
        int pos1 = m - 1, pos2 = n - 1, cur = m + n - 1, minnum = -1000000;
        while (pos1 >= 0 || pos2 >= 0) {
            int val1 = pos1 < 0 ? minnum : nums1[pos1], val2 = pos2 < 0 ? minnum : nums2[pos2];
            nums1[cur--] = val1 > val2 ? nums1[pos1--] : nums2[pos2--];
        }
    }
};
```