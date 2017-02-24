title: LeetCode22. Generate Parentheses
date: 2017-02-24 23:30:18
tags:
- 数据结构
- 算法
---

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

#### 递归来拼字符串；左括号的数量小于n时，可继续添加左括号；右括号的数量小于左括号时，可添加右括号

```
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> result;
        helper(0, 0, n, "", result);
        return result;
    }
    
    void helper(int left, int right, int n, string s, vector<string> &result) {
        if (right == n) { 
            result.push_back(s);
            return ;
        }
        
        if (left < n) {
            helper(left + 1, right, n, s + '(', result);
        }
        
        if (right < left) {
            helper(left, right + 1, n, s + ')', result);
        }
    }
};
```