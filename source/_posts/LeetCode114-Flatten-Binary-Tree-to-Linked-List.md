title: LeetCode114. Flatten Binary Tree to Linked List
date: 2017-12-01 16:16:51
tags:
- 数据结构
- 算法
---

Given a binary tree, flatten it to a linked list in-place.

For example,
Given

```
         1
        / \
       2   5
      / \   \
     3   4   6
```

The flattened tree should look like:

```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

二叉树转链表

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
    
    void helper(TreeNode* &root, TreeNode* &last) {
        last = root;
        if (root == nullptr)
            return ;
        
        TreeNode *mylast, *temp = root->right;
        helper(root->left, mylast);
        if (mylast) {
            last = mylast;
            last->right = root->right;
            root->right = root->left;
            root->left = nullptr;
        }
        
        helper(root->right, mylast);
        if (mylast)
            last = mylast;
        
    }
    
    void flatten(TreeNode* root) {
        TreeNode *last;
        helper(root, last);
    }
};
```