title: LeetCode268. Missing Number
date: 2017-02-16 16:34:32
tags:
- 数据结构
- 算法
---

Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

For example,
Given nums = [0, 1, 3] return 2.

Note:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int s = nums.size();
        nums[s] = -1;
        int i = 0;
        while (i < s) {
            if (nums[i] != -1 && nums[i] != i) {
                swap(nums[i], nums[nums[i]]);
            } else {
                i++;
            }
        }
        
        int result = nums.size();
        for (int j = 0; j < nums.size(); j++) {
            if (nums[j] == -1) {
                result = j;
                break;
            }
        }
        
        return result;
    }
    
    void swap(int &a, int &b) {
        a = a ^ b;
        b = a ^ b;
        a = a ^ b;
    }
};
```
