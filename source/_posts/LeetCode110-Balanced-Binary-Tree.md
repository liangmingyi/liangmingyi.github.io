title: LeetCode110. Balanced Binary Tree
date: 2017-02-14 23:31:42
tags:
- 数据结构
- 算法
---

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.


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
    bool isBalanced(TreeNode* root) {
        if (root == NULL)
            return true;
        if (!isBalanced(root->left) || !isBalanced(root->right)) return false;
        
        int leftMaxDepth = maxDepth(root->left);
        int rightMaxDepth = maxDepth(root->right);
        return abs(leftMaxDepth - rightMaxDepth) <= 1;
    }
    
    
    int maxDepth(TreeNode* root) {
        if (root == NULL)
            return 0;
        int leftDepth = root->left ? maxDepth(root->left) : 0;
        int rightDepth = root->right ? maxDepth(root->right) : 0;
        return max(leftDepth, rightDepth) + 1;
    }
};
```