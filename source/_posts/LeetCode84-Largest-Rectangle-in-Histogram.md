title: LeetCode84. Largest Rectangle in Histogram
date: 2017-11-28 13:53:54
tags:
- 数据结构
- 算法
---

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![](https://leetcode.com/static/images/problemset/histogram.png)

Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.

![](https://leetcode.com/static/images/problemset/histogram_area.png)

The largest rectangle is shown in the shaded area, which has area = `10` unit.

For example,
Given heights = `[2,1,5,6,2,3]`,
return 10.

栈顶元素比新元素小时，入栈，确定左边界，当栈顶元素比新元素大时，出栈，确定右边界，并计算此时的面积，顺带比较最大值。时间复杂度O(n)

```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size(), result = 0, h;
        stack<int> s;
        for (int i = 0; i < n; i++) {
            while(!s.empty() && heights[s.top()] > heights[i]) {
                h = heights[s.top()];
                s.pop();
                result = max(result, (i - 1 - (s.empty()?(-1):s.top())) * h);
            }
            s.push(i);
        }
        
        while(!s.empty()) {
            h = heights[s.top()];
            s.pop();
            result = max(result, (n - 1 - (s.empty()?(-1):s.top())) * h);
        }
        
        return result;
    }
};
```