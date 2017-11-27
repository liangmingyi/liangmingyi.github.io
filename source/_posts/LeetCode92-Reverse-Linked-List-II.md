title: LeetCode92. Reverse Linked List II
date: 2017-11-27 20:42:06
tags:
- 数据结构
- 算法
---

Reverse a linked list from position m to n. Do it in-place and in one-pass.

For example:
Given `1->2->3->4->5->NULL`, m = 2 and n = 4,

return `1->4->3->2->5->NULL`.

Note:
Given m, n satisfy the following condition:
1 ≤ m ≤ n ≤ length of list.

链表的操作，不难，就是比较繁琐，在纸上先画出操作步骤理清思路。

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
    ListNode* reverseLinklist(ListNode* head) {
        ListNode *p1 = head, *p2 = head->next, *tmp;
        while (p2) {
            tmp = p2->next;
            p2->next = p1;
            p1 = p2;
            p2 = tmp;
        }
        head->next = nullptr;
        return p1;
    }
    
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        if (head == nullptr || head->next == nullptr || m == n)
            return head;
        ListNode *p1 = head, *dummy = new ListNode(-1), *p2 = dummy, *p3, *p4, *p5;
        dummy->next = head;
        int i = 1;
        while (p1) {
            if (i == m && p2) {
                p2->next = nullptr;
                p3 = p1;
            } else if (i < m) {
                p2 = p1;
            }
            
            if (i == n) {
                p4 = p1->next;
                p1->next = nullptr;
                break;
            }
            
            p1 = p1->next;
            i++;     
        }

        p5 = reverseLinklist(p3);
        p2->next = p5;
        p3->next = p4;
        return dummy->next;
    }
};
```