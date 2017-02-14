title: LeeCode104. Maximum Depth of Binary Tree
date: 2017-02-14 22:56:02
tags: 
- 数据结构
- 算法
---

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

递归求左右子树的高度取最大再加1即为所求

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
    int maxDepth(TreeNode* root) {
        if (root == NULL)
            return 0;
        int leftHeight = root->left ? maxDepth(root->left) : 0, rightHeight = root->right ? maxDepth(root->right) : 0;
        return max(leftHeight, rightHeight) + 1;
    }
};
```