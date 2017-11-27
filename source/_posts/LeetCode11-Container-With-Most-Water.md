title: LeetCode11. Container With Most Water
date: 2017-02-20 22:59:33
tags:
- 数据结构
- 算法
---

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

#### 两头扫，贪心法 时间复杂度O(n)
```
class Solution {
public:
    int maxArea(vector<int>& height) {
        int s = height.size(), left = 0, right = s - 1, result = 0;
        while (left < right) {
            int h = min(height[left], height[right]);
            result = max(result, h * (right - left));
            while (height[left] <= h && left < right) { left++; }
            while (height[right] <= h && right > left) { right--; }
        }
        
        return result;
    }
};
```
