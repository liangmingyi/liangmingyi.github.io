title: LeetCode283. Move Zeroes
date: 2017-02-16 17:31:11
tags:
- 数据结构
- 算法
---

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.

```
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int s = nums.size(), cur = 0;
        for (int i = 0; i < s; i++) {
            if (nums[i] != 0) {
                nums[cur++] = nums[i];
            }
        }
        
        while (cur < s) {
            nums[cur++] = 0;
        }
    }
};
```
