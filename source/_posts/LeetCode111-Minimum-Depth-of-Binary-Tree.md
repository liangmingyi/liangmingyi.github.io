title: LeetCode111. Minimum Depth of Binary Tree
date: 2017-11-30 16:31:48
tags:
- 数据结构
- 算法
---

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.


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
class Solution {
public:
    int minDepth(TreeNode* root) {
        int ret = 0;
        if (root == nullptr)
            return 0;
        
        if (root->left) {
            if (root->right) {
                ret = min(minDepth(root->left), minDepth(root->right)) + 1; 
            } else {
                ret = minDepth(root->left) + 1;
            }
        } else if (root->right) {
            ret = minDepth(root->right) + 1;
        } else {
            return 1;
        }
        
        return ret;
    }
};
```
