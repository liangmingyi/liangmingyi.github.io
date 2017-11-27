title: LeetCode16. 3Sum Closest
date: 2017-02-20 23:35:42
tags:
- 数据结构
- 算法
---

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

```
    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

#### 除了O(N3)的穷举解法外，还可以通过对数组排序，假设第一个数已经确定，第二个数从左边扫，第三个数从右边扫，然后根据当前和与目标值比较的结果调整第二个或第三个数的位置，时间复杂度O(N2)

```
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if (nums.size() < 4)
            return accumulate(nums.begin(), nums.end(), 0);
        
        sort(nums.begin(), nums.end());
        int s = nums.size(), result = 10000000;
        for (int i = 0; i < s; i++) {
            int j = i + 1;
            int k = s - 1;
            
            while (j < k) {
                int tmpSum = nums[i] + nums[j] + nums[k];
                if (abs(tmpSum - target) < abs(result - target)) {
                    result = tmpSum;
                }
                
                if (tmpSum > target) {
                    k--;
                } else if (tmpSum < target) {
                    j++;
                } else {
                    result = target;
                    i = s; // break out iterator
                    break;
                }
            }
        }
        
        return result;
    }
};
```