title: LeetCode39. Combination Sum
date: 2017-02-25 23:48:50
tags:
- 数据结构
- 算法
---

Given a set of candidate numbers (C) (without duplicates) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set `[2, 3, 6, 7]` and target 7, 
A solution set is: 

```
[
  [7],
  [2, 2, 3]
]
```

#### 跟[LeetCode22. Generate Parentheses](http://mingyi.js.org/2017/02/24/LeetCode22-Generate-Parentheses/)类似，递归

```
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        sort(candidates.begin(), candidates.end());
        
        vector<int> path;
        DFS(candidates, target, 0, path, result);
        return result;
    }
    
    void DFS(vector<int>& candidates, int target, int start, vector<int>& path, vector<vector<int>>& result) {
        if (target == 0) {
            result.push_back(path);
            return ;
        }
        int len = candidates.size();
        for (int i = start; i < len; i++) {
            if (target < candidates[i]) {
                return;
            }
            
            path.push_back(candidates[i]);
            DFS(candidates, target - candidates[i], i, path, result);
            path.pop_back();
        }
    }
};
```