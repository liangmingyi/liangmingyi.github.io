title: LeetCode106. Construct Binary Tree from Inorder and Postorder Traversal
date: 2017-11-29 17:33:26
tags:
- 数据结构
- 算法
---

Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

已知树的中序和后序遍历，构建这棵树，跟105思路一样，比较简单

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
    TreeNode* helper(vector<int>& inorder, vector<int>& postorder, int inpos, int postpos, int len) {
        if (len == 0)
            return nullptr;
        
        TreeNode* root = new TreeNode(postorder[postpos + len -1]);
        int i = 0;
        for (; inorder[i + inpos] != postorder[postpos + len -1]; i++) ;
        root->left = helper(inorder, postorder, inpos, postpos, i);
        root->right = helper(inorder, postorder, inpos + i + 1, postpos + i, len - i - 1);
        return root;
    }

    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        return helper(inorder, postorder, 0, 0, postorder.size());
    }
};
```