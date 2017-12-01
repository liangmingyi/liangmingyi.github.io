title: LeetCode109. Convert Sorted List to Binary Search Tree
date: 2017-12-01 18:14:54
tags:
- 数据结构
- 算法
---

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.


Example:

```
Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
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
    
    int getLen(ListNode* head) {
        int len = 0;
        while (head) {
            head = head->next;
            len++;
        }
        return len;
    }
    
    void helper(ListNode* head, int len, TreeNode* &root) {
        if (len == 0) {
            root = nullptr;
            return ;
        }

        ListNode* now = head;
        for (int i = (len - 1) >> 1; i > 0; i--) {
            now = now->next;
        }
        root = new TreeNode(now->val);
        TreeNode *left, *right;
        helper(head, (len - 1) >> 1, left);
        helper(now->next, len >> 1, right);
        root->left = left;
        root->right = right;
    }
    
    TreeNode* sortedListToBST(ListNode* head) {
        TreeNode* root;
        helper(head, getLen(head), root);
        return root;
    }
};
```
