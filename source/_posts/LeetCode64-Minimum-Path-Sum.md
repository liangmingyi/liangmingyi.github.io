title: LeetCode64. Minimum Path Sum
date: 2017-12-02 18:55:20
tags:
- 数据结构
- 算法
---

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

Example 1:

```
[[1,3,1],
 [1,5,1],
 [4,2,1]]
 ```

Given the above grid map, return 7. Because the path 1→3→1→1→1 minimizes the sum.


动态规划问题，先确定状态转移方程，dp[i][j]表示到下标为i,j的位置时的最优解

```
dp[i][j] = min(dp[i][j - 1], dp[i - 1][j]) + grid[i][j]
```

初值

```
dp[0][0] = grid[0][0]
dp[0][j > 0] = dp[0][j - 1] + grid[i][j] // 第一行只能横着走
dp[i > 0][0] = dp[i - 1][0] + grid[i][j] // 最左列只能竖着走
```

时间复杂度O(mn)，空间复杂度O(mn)，可以通过滚动数组优化把空间复杂度降维到O(n)

```
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n));
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0) {
                    if (j == 0) {
                        dp[i][j] = grid[0][0];
                    } else {
                        dp[i][j] = dp[i][j - 1] + grid[i][j];
                    }
                } else if (j == 0) {
                    dp[i][j] = dp[i - 1][j] + grid[i][j];
                } else {
                    dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
                }
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

