title: LeetCode2. Add Two Numbers
date: 2017-02-14 01:21:48
tags: 数据结构, 算法 
---

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8

大数相加

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode head(-1), *current = &head;
        int extra = 0, l1val = 0, l2val = 0, sum;
        while (l1 || l2) {
            l1val = 0, l2val = 0;
            if (l1) {
                l1val = l1->val;
                l1 = l1->next;
            }
            if (l2) {
                l2val = l2->val;
                l2 = l2->next;
            }
            sum = l1val + l2val + extra;
            ListNode *n = new ListNode(sum % 10);
            extra = sum / 10;
            current->next = n;
            current = n;
        }
        if (extra > 0) {
            current->next = new ListNode(extra);
        }
        return head.next;
    }
};
```