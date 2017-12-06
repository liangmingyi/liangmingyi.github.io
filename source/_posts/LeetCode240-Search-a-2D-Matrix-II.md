title: LeetCode240. Search a 2D Matrix II
date: 2017-12-06 17:37:46
tags:
- 数据结构
- 算法
---

[问题来自LeetCode240](https://leetcode.com/problems/search-a-2d-matrix-ii/)

比较取巧的一个方法是选择右上角的元素matrix[i][j]和target作比较，如果比target大，则一定不在最后一列，如果比target下，则一定不在第一行，如果相等，则找到了。


```
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        if (m == 0)
            return false;
        
        int n = matrix[0].size(), i = 0, j = n - 1;
        while (i < m && j >= 0) {
            if (matrix[i][j] == target)
                return true;
            else if (matrix[i][j] < target)
                i++;
            else 
                j--;
        }
        return false;
    }
};
```

另一种方法是用分治的思想，选取中心点元素，如果正好与target相等，则找到了，如果比target小，则target一定不在第一象限的子矩阵里，如果比target大，则target一定不在第四象限的子矩阵里。排除第一或第四象限的子矩阵后，还剩一个原来1/4的矩阵和一个1/2的子矩阵，分别再在子矩阵中递归该过程，直到找到结果

```
class Solution {
public:
    
    bool find(vector<vector<int>>& matrix, int x1, int y1, int x2, int y2, int target) {
        if (x1 > x2 || y1 > y2)
            return false;
        
        int midx = (x1 + x2) >> 1;
        int midy = (y1 + y2) >> 1;
        
        if (target == matrix[midx][midy])
            return true;
        return matrix[midx][midy] > target ? (find(matrix, x1, y1, x2, midy - 1, target) || find(matrix, x1, midy, midx - 1, y2, target)) : (find(matrix, midx + 1, y1, x2, midy, target) || find(matrix, x1, midy + 1, x2, y2, target));
    }
    
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        if (m == 0)
            return false;
        
        int n = matrix[0].size();
        return find(matrix, 0, 0, m - 1, n - 1, target);
    }
};
```