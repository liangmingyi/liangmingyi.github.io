title: LeeCode350. Intersection of Two Arrays II
date: 2017-02-18 17:03:38
tags:
- 数据结构
- 算法
---

Given two arrays, write a function to compute their intersection.

Example:
Given nums1 = [1, 2, 2, 1], nums2 = [2, 2], return [2, 2].

Note:
Each element in the result should appear as many times as it shows in both arrays.
The result can be in any order.
Follow up:
What if the given array is already sorted? How would you optimize your algorithm?
What if nums1's size is small compared to nums2's size? Which algorithm is better?
What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?


```
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> result;
        unordered_map<int, int> m;
        
        for (int item : nums1)
            m[item]++;
        
        for (int item : nums2) 
            if (m[item]-- > 0) 
                result.push_back(item);

        return result;
    }
};
```
