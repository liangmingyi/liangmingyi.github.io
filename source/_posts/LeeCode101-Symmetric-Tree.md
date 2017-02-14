title: LeeCode101. Symmetric Tree
date: 2017-02-14 20:36:36
tags: 数据结构, 算法
---

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

    1
   / \
  2   2
 / \ / \
3  4 4  3
But the following [1,2,2,null,3,null,3] is not:
    1
   / \
  2   2
   \   \
   3    3
Note:
Bonus points if you could solve it both recursively and iteratively.


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
    bool isSymmetric(TreeNode* root) {
        if (root == NULL)
            return true; 
        
        return help(root->left, root->right);
    }
    
    bool help(TreeNode* p1, TreeNode* p2) {
        if (p1 == NULL && p2 == NULL)
            return true;
        if (p1 == NULL || p2 == NULL)
            return false;
        
        return p1->val == p2->val && help(p1->left, p2->right) && help(p1->right, p2->left);
    }
};
```