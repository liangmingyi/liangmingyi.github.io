title: 最大子序列和
date: 2016-12-02 21:14:16
tags: 数据结构, 算法
---

```
//
//  main.c
//
//  Created by Mingyi on 2016/11/30.
//  Copyright © 2016年 Mingyi Liang. All rights reserved.
//

#include <stdio.h>
#include <time.h>

int solution1(int a[], int N); // O(N3)
int solution2(int a[], int N); // O(N2)
int solution3(int a[], int N); // O(N)

int main() {
    int a[] = {1, 2, 3, -4, 5};
    time_t tstart, tend;
    tstart = clock();
    int result = solution1(a, 5);
    for (int c = 0; c < 1000000; c++) {
        solution1(a, 5);
    }
    tend = clock();
    printf("result is: %d\n", result);
    printf("cost time: %f\n", (double)(tend - tstart) / CLOCKS_PER_SEC);
    return 0;
}

int solution1(int a[], int N) {
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

int solution2(int a[], int N) {
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


int solution3(int a[], int N) {
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