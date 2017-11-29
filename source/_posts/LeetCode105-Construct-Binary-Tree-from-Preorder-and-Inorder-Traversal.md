title: LeetCode105. Construct Binary Tree from Preorder and Inorder Traversal
date: 2017-11-29 17:15:20
tags:
- 数据结构
- 算法
---

Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

已知树的先序和中序遍历，构建这棵树。比较简单

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
    
    TreeNode* helper(vector<int>& preorder, vector<int>& inorder, int prepos, int inpos, int len) {
        if (len == 0)
            return nullptr;
        
        TreeNode* root = new TreeNode(preorder[prepos]);
        int i = 0;
        for (; inorder[i + inpos] != preorder[prepos]; i++) ;
        root->left = helper(preorder, inorder, prepos + 1, inpos, i);
        root->right = helper(preorder, inorder, prepos + i + 1, inpos + i + 1, len - i - 1);
        
        return root;
    }
    
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return helper(preorder, inorder, 0, 0, inorder.size());
    }
};
```
