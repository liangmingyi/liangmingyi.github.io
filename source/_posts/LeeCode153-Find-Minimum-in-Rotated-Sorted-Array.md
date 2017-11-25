title: LeeCode153. Find Minimum in Rotated Sorted Array
date: 2017-11-25 17:43:45
tags:
- 数据结构
- 算法
---

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.


二分的思想

```
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0, right = nums.size() - 1, mid, ret;
        if (left == right)
            return nums[0];
        
        while (left < right) {
            mid = (left + right) / 2;
            if (right - left == 1) {
                ret = min(nums[left], nums[right]);
                break;
            }
            
            if (nums[left] < nums[right]) {
                ret = nums[left];
                break;
            }
            
            if (right - left == 2 && nums[mid] > nums[right]) {
                ret = nums[right];
                break;
            }
            
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        return ret;
    }
};
```