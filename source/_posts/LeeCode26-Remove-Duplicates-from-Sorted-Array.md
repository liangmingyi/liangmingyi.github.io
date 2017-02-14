title: LeeCode26. Remove Duplicates from Sorted Array
date: 2017-02-14 19:20:04
tags: 数据结构 算法
---

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.


```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int s = nums.size();
        if (s < 2)
            return s;
        int cur = 0, pos;
        for (pos = 1; pos < s; pos++) {
            if (nums[pos] != nums[cur])
                nums[++cur] = nums[pos];
        }
        
        return cur + 1;
    }
};
```
