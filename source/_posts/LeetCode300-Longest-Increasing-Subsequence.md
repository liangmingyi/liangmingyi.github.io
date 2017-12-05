title: LeetCode300. Longest Increasing Subsequence
date: 2017-12-05 23:49:36
tags:
- 数据结构
- 算法
---

[问题来自LeetCode300](https://leetcode.com/problems/longest-increasing-subsequence)

dp[i]表示以第i位结尾的长度，则状态转移方程dp[i] = max(d[j] + 1) && i > j && nums[i] > nums[j]
初值 dp[i] = 1，时间复杂度O(n2)

```
class Solution {
public:
    
    int lengthOfLIS(vector<int>& nums) {
        int len = nums.size(), ret = 1;
        vector<int> dp(len, 1); 
        
        if (len == 0)
            return 0;
        
        for (int i = 0; i < len; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j])
                    dp[i] = max(dp[i], dp[j] + 1);
            }
            ret = max(ret, dp[i]);
        }
        
        return ret;
    }
};
```