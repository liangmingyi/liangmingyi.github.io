title: LeetCode18. 4Sum
date: 2017-02-22 00:00:23
tags:
- 数据结构
- 算法
---

Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note: The solution set must not contain duplicate quadruplets.

For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

```
A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

#### 3sum的最优解时间复杂度是O(N2)的，4sum如果建立在3sum之上，最优解的时间复杂度是O(N3)，实现代码如下

```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        int s = nums.size();
        if (s < 4) {
            return result;
        }
        if (s == 4) {
            if (accumulate(nums.begin(), nums.end(), 0) == target) {
                result.push_back(nums);
            }
            return result;
        }
        
        sort(nums.begin(), nums.end());
        
        int first = 0, second = 0, third = second + 1, fourth = s - 1;
        for (first = 0; first < s - 3; first ++) {
            for (second = first + 1; second < s - 2; second ++) {
                third = second + 1;
                fourth = s - 1;
                while (third < fourth) {
                    int tmpSum = nums[first] + nums[second] + nums[third] + nums[fourth];
                    if (tmpSum == target) {
                        vector<int> tmp;
                        tmp.push_back(nums[first]);
                        tmp.push_back(nums[second]);
                        tmp.push_back(nums[third]);
                        tmp.push_back(nums[fourth]);
                        result.push_back(tmp);
                        
                        do {
                            third ++;
                        } while (third < fourth && nums[third] == nums[third - 1]);
                        do {
                            fourth --;
                        } while (third < fourth && nums[fourth] == nums[fourth + 1]);
                        
                    } else if (tmpSum < target) {
                        third ++;
                    } else {
                        fourth --;
                    }
                }
                
                while (second < s - 2 && nums[second] == nums[second + 1]) {
                    second ++;
                }
            }
            
            while (first < s - 3 && nums[first] == nums[first + 1]) {
                first ++;
            }
        }
        
        return result;
    }
};
```
