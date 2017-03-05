title: LeetCode70. Climbing Stairs
date: 2017-03-05 22:20:32
tags:
- 数据结构
- 算法
---

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.


#### 排列组合问题，可用递归求解；但递归会消耗系统栈空间，在leetcode上提交的时候 在n=44的这个case时，内存会爆。所以还需要对尾递归优化，用循环代替~

```
class Solution {
public:
    int climbStairs(int n) {
        if (n == 0 || n == 1 || n == 2)
            return n;
        
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
};
```

#### 优化后:
```
class Solution {
public:
    int climbStairs(int n) {
        if (n == 0 || n == 1 || n == 2)
            return n;
        
        int result[n];
        result[0] = 1;
        result[1] = 2;
        for (int i = 2; i < n; i++)
            result[i] = result[i - 1] + result[i - 2];
        
        return result[n - 1];
    }
};
```