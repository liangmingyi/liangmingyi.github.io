title: LeeCode19. Remove Nth Node From End of List
date: 2017-02-22 00:26:42
tags:
- 数据结构
- 算法
---

Given a linked list, remove the nth node from the end of list and return its head.

For example,
```
   Given linked list: 1->2->3->4->5, and n = 2.

   After removing the second node from the end, the linked list becomes 1->2->3->5.
```

#### 链表的操作，对头尾进行删除时需要特殊处理，也可采用在头部再增加一个节点，就只需要对删尾节点做特殊处理

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int len = getLength(head);
        if (len == 0 || n < 1)
            return head;
        
        if (len == n) {
            if (len == 1) {
                return NULL;
            } else {
                return head->next;
            }
        }
        
        ListNode* p = head;
        for (int i = 0; i < len - n - 1; i++) {
            p = p->next;
        }
        ListNode* deleted = p->next;
        p->next = (n == 1 ? NULL : deleted->next);
        delete deleted;
        return head;
    }
    
    int getLength(ListNode* head) {
        int len = 0;
        while (head) {
            len ++;
            head = head->next;
        }
        return len;
    }
};
```