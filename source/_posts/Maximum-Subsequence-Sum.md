title: [LeetCode]53. Maximum Subarray
date: 2016-12-02 21:14:16
tags: 数据结构, 算法
---

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.

穷举法 时间复杂度O(N3)
```
int solution(int a[], int N) {
    int x, y, maxSum = 0;
    for (x = 0; x < N; x++) {
        for (y = x; y < N; y++ ) {
            int k, thisSum = 0;
            for (k = x; k <= y; k++) {
                thisSum += a[k];
            }
            if (thisSum > maxSum) {
                maxSum = thisSum;
            }
        }
    }
    return maxSum;
}
```


穷举法优化冗余的求和步骤 时间复杂度O(N2)
```
int solution(int a[], int N) {
    int x, y, maxSum = 0;
    for (x = 0; x < N; x++) {
        int thisSum = 0;
        for (y = x; y < N; y++) {
            thisSum += a[y];
            if (thisSum > maxSum) {
                maxSum = thisSum;
            }
        }
    }
    return maxSum;
}
```

贪心法 时间复杂度O(N)
```
int solution(int a[], int N) {
    int x, maxSum = 0, thisSum = 0;
    for (x = 0; x < N; x++) {
        thisSum += a[x];
        if (thisSum > maxSum) {
            maxSum = thisSum;
        }
        if (thisSum < 0) {
            thisSum = 0;
        }
    }
    return maxSum;
}

```