title: LeeCode374. Guess Number Higher or Lower
date: 2017-02-18 18:08:43
tags:
- 数据结构
- 算法
---

We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API guess(int num) which returns 3 possible results (-1, 1, or 0):
```
-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!
```

Example:
```
n = 10, I pick 6.

Return 6.
```

```
// Forward declaration of guess API.
// @param num, your guess
// @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
int guess(int num);

class Solution {
public:
    int guessNumber(int n) {
        int left = 1, right = n, mid = (left + right) / 2, tmp;
        while (left <= right) {
            mid = (left + right) / 2;
            tmp = guess(mid);
            if (tmp == 1) {
                left = mid + 1;
            } else if (tmp == -1) {
                right = mid -1;
            } else {
                break;
            }
        }

        return mid;
    }
};
```