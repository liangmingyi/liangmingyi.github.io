title: LeetCode108. Convert Sorted Array to Binary Search Tree
date: 2017-12-02 17:25:09
tags:
- 数据结构
- 算法
---

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.


Example:

```
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

数组转BST，比链表要简单

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
typedef vector<int>::iterator vec_iter;
class Solution {
public:
    
    void helper(TreeNode* &root, vec_iter first, vec_iter last) {
        int len = last - first;
        if (len == 0)
            return ;
        
        int mid = (len - 1) >> 1;
        root = new TreeNode(*(first + mid));
        
        TreeNode *myleft = nullptr, *myright = nullptr;
        helper(myleft, first, first + mid);
        helper(myright, first + mid + 1, last);
        root->left = myleft;
        root->right = myright;
    }
    
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        TreeNode* root = nullptr;
        helper(root, nums.begin(), nums.end());
        return root;
    }
};
```
