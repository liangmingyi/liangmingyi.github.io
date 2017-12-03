title: LeetCode739. Daily Temperatures
date: 2017-12-03 18:46:42
tags:
- 数据结构
- 算法
---

Given a list of daily temperatures, produce a list that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead.

For example, given the list temperatures = [73, 74, 75, 71, 69, 72, 76, 73], your output should be [1, 1, 4, 2, 1, 1, 0, 0].

Note: The length of temperatures will be in the range [1, 30000]. Each temperature will be an integer in the range [30, 100].


比较简单，利用栈来做，如果后一个数比前一个数小，则入栈，否则出栈。入栈的同时顺带计数

```
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int len = temperatures.size();
        vector<int> ret(len);
        
        if (len == 0)
            return ret;
        
        stack<int> s;
        
        for (int i = 0; i < len; i++) {
            int tmp = 0;
            while (!s.empty() && temperatures[i] > temperatures[s.top()]) {
                tmp = ret[s.top()];
                s.pop();
                
                if (!s.empty()) 
                    ret[s.top()] += tmp;
            }
            
            s.push(i);
            ret[s.top()] ++;
        }
        
        while(!s.empty()) {
            ret[s.top()] = 0;
            s.pop();
        }
            
        
        return ret;
    }
};
```