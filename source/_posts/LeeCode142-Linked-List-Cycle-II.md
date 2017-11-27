title: LeeCode142. Linked List Cycle II
date: 2017-11-27 18:57:09
tags:
- 数据结构
- 算法
---

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.

Follow up:
Can you solve it without using extra space?


笨办法就是把每个节点的地址塞入一个set里，每次set之前先find，第一次find到的那就是环的起始位置，这种方法需要额外的set容器。另一种做法仍然是用两根指针，p1每次走一步，p2每次走两步，当p1与p2相遇时，p1走过a步，p2走过b步。b = 2a; b - a = n * k; n 为p2在环内转的圈数，k为环的大小，则a = n * k；将p1放回起点，与p2再一起每次走一步，再次遇到的点即为换的起点。

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
    ListNode *detectCycle(ListNode *head) {
        ListNode *p1 = head, *p2 = head, *ret;
        while (p1 && p2) {
            p1 = p1->next;
            p2 = p2->next;
            
            if (p2 == nullptr || p2->next == nullptr) {
                ret = nullptr;
                return ret;
            }
            
            p2 = p2->next;
            if (p1 == p2) {
                break;
            }
        }
        
        if (p1 == head) {
            return p1;
        }
        
        p1 = head;
        while (p1 && p2) {
            p1 = p1->next;
            p2 = p2->next;
            
            if (p1 == p2) {
                ret = p1;
                break;
            }
        }
        return ret;
    }
};
```