title: LeetCode24. Swap Nodes in Pairs
date: 2017-03-10 10:52:43
tags:
- 数据结构
- 算法
---

Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given `1->2->3->4`, you should return the list as `2->1->4->3`.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

链表的操作

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
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr)
            return head;
        
        // 在头增加一个节点
        ListNode pre = ListNode(-1);
        pre.next = head;
        
        ListNode *t1 = &pre, *t2 = t1->next, *t3 = t2->next, *t4 = t3->next;
        while (t3 != nullptr) {
            t1->next = t3;
            t3->next = t2;
            t2->next = t4;
            
            t1 = t2;
            t2 = t4;
            t3 = t2 ? t2->next : nullptr;
            t4 = t3 ? t3->next : nullptr;
        }
        
        return pre.next;
    }
};
```