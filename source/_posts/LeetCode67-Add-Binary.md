title: LeetCode67. Add Binary
date: 2017-03-02 17:09:22
tags:
- 数据结构
- 算法
---

Given two binary strings, return their sum (also a binary string).

For example,
a = `"11"`
b = `"1"`
Return `"100"`.

```
class Solution
{
public:
    string addBinary(string a, string b)
    {
        string result = "";
        
        int sum = 0, i = a.length() - 1, j = b.length() - 1;
        while(i >= 0 || j >= 0 || sum == 1)
        {
            sum += i >= 0 ? a[i --] - '0' : 0;
            sum += j >= 0 ? b[j --] - '0' : 0;
            result = char(sum % 2 + '0') + result;
            sum /= 2;
        }
        
        return result;
    }
};
```


