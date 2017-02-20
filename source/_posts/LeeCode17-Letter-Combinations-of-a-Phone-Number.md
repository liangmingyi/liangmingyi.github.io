title: LeeCode17. Letter Combinations of a Phone Number
date: 2017-02-21 00:42:28
tags:
- 数据结构
- 算法
---

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

![Alt text](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

```
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

#### 递归，第一个字符和其余字符的结果的组合为最后的结果

```
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> result;
        int len = digits.length();
        if (len == 0)
            return result;
        
        int num = digits[0] - '0';
        string val = valueOf(num);
        
        if (len == 1) {
            for (int i = 0, j = val.length(); i < j; i++) {
                result.push_back(string(1, val[i]));
            }
        } else {
            vector<string> otherResult = letterCombinations(digits.substr(1));
            for (int i = 0, j = val.length(); i < j; i++) {
                for (int l = 0, m = otherResult.size(); l < m; l++) {
                    result.push_back(string(1, val[i]) + otherResult[l]);
                }
            }
        }
        
        return result;
    }
    
    string valueOf(int num) {
        string result;
        switch(num) {
            case 2: result = "abc"; break;
            case 3: result = "def"; break;
            case 4: result = "ghi"; break;
            case 5: result = "jkl"; break;
            case 6: result = "mno"; break;
            case 7: result = "pqrs"; break;
            case 8: result = "tuv"; break;
            case 9: result = "wxyz"; break;
            default: result = "";
        }
        
        return result;
    }
};
```