title: LeetCode349. Intersection of Two Arrays
date: 2017-02-17 11:44:48
tags:
- 数据结构
- 算法
---

Given two arrays, write a function to compute their intersection.

Example:
Given nums1 = [1, 2, 2, 1], nums2 = [2, 2], return [2].

Note:
Each element in the result must be unique.
The result can be in any order.

```
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> s(nums1.begin(), nums1.end());
        vector<int> result;
        for (auto item : nums2) {
            if (s.count(item)) {
                result.push_back(item);
                s.erase(item);
            }
        }
        
        return result;
    }
};
```
