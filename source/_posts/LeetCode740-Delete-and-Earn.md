title: LeetCode740. Delete and Earn
date: 2017-12-04 11:20:32
tags:
- 数据结构
- 算法
---

Given an array nums of integers, you can perform operations on the array.

In each operation, you pick any nums[i] and delete it to earn nums[i] points. After, you must delete every element equal to nums[i] - 1 or nums[i] + 1.

You start with 0 points. Return the maximum number of points you can earn by applying such operations.

Example 1:

```
Input: nums = [3, 4, 2]
Output: 6
Explanation: 
Delete 4 to earn 4 points, consequently 3 is also deleted.
Then, delete 2 to earn 2 points. 6 total points are earned.
```

Example 2:

```
Input: nums = [2, 2, 3, 3, 3, 4]
Output: 9
Explanation: 
Delete 3 to earn 3 points, deleting both 2's and the 4.
Then, delete 3 again to earn 3 points, and 3 again to earn 3 points.
9 total points are earned.
```

Note:

The length of nums is at most 20000.
Each element nums[i] is an integer in the range [1, 10000].


动态规划问题，状态转移方程
dp[i] = max(dp[i - 1], dp[i - 2] + a[i])

初值
dp[1] = a[1]
dp[2] = max(dp[1], a[2] * 2)


```
class Solution {
public:
    
    int deleteAndEarn(vector<int>& nums) {
        int n = 10001;
        vector<int> a(n, 0);
        for (int i : nums) 
            a[i]++;
        
        int dp[10001];
        dp[1] = a[1];
        dp[2] = max(dp[1], a[2] * 2);
        
        for (int i = 3; i < n; i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + a[i] * i);
        }
        
        return dp[10000];
    }
};
```