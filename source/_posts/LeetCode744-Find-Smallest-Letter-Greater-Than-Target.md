title: LeetCode744. Find Smallest Letter Greater Than Target
date: 2017-12-10 17:43:41
tags:
- 数据结构
- 算法
---

[问题来自LeetCode744](https://leetcode.com/problems/find-smallest-letter-greater-than-target)

Given a list of sorted characters letters containing only lowercase letters, and given a target letter target, find the smallest element in the list that is larger than the given target.

Letters also wrap around. For example, if the target is target = 'z' and letters = ['a', 'b'], the answer is 'a'.

Examples:

```
Input:
letters = ["c", "f", "j"]
target = "a"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "c"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "d"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "g"
Output: "j"

Input:
letters = ["c", "f", "j"]
target = "j"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "k"
Output: "c"
```

Note:
letters has a length in range [2, 10000].
letters consists of lowercase letters, and contains at least 2 unique letters.
target is a lowercase letter.


二分

```
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int len = letters.size();
        if (target < letters[0] || target >= letters[len - 1])
            return letters[0];
        int left = 0, right = len - 1, mid;
        while (left < right) {
            mid = (left + right) >> 1;
            if (letters[mid] > target) {
                right = mid;
            }
            
            if (letters[mid] <= target) {
                left = mid + 1;
            }
        }
        
        if (letters[mid] <= target)
            return letters[mid + 1];
        else if (letters[mid - 1] > target){
            return letters[mid - 1];
        } else {
            return letters[mid];
        }
    }
};
```
