title: LeetCode98. Validate Binary Search Tree
date: 2017-11-30 18:17:26
tags:
- 数据结构
- 算法
---

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
Example 1:

```
    2
   / \
  1   3
```

Binary tree `[2,1,3]`, return true.
Example 2:

```
    1
   / \
  2   3
```

Binary tree `[1,2,3]`, return false.

将BST的中序遍历的结果存起来然后判断是否有序

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
    
    void travel(TreeNode* root, stack<int> &s) {
        if (root == nullptr)
            return;
        travel(root->left, s);
        s.push(root->val);
        travel(root->right, s);
    }
    
    bool isValidBST(TreeNode* root) {
        if (root == nullptr)
            return true;
        
        stack<int> s;
        travel(root, s);
        
        int last = s.top();
        s.pop();
        
        while(!s.empty()) {
            if (s.top() >= last)
                return false;
            last = s.top();
            s.pop();
        }
        return true;
    }
};
```