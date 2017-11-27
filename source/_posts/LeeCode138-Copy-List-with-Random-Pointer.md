title: LeeCode138. Copy List with Random Pointer
date: 2017-11-27 18:21:00
tags:
- 数据结构
- 算法
---

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.


带有一个random指针链表的拷贝。先在每个节点后面复制一个节点，然后再将复制的新节点的random指向新的节点。最后将新链表分离出来

```
/**
 * Definition for singly-linked list with a random pointer.
 * struct RandomListNode {
 *     int label;
 *     RandomListNode *next, *random;
 *     RandomListNode(int x) : label(x), next(NULL), random(NULL) {}
 * };
 */
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
        if (head == nullptr)
            return nullptr;
            
        RandomListNode *p = head, *tmp;
        while (p) {
            RandomListNode *n = new RandomListNode(p->label); 
            tmp = p->next;
            p->next = n;
            n->next = tmp;
            p = tmp;
        }
        
        p = head;
        while (p && p->next) {
            p->next->random = p->random == nullptr ? nullptr : p->random->next;
            p = p->next->next;
        }
        
        p = head;
        RandomListNode *h = p->next, *t = h, *tail = p;
        while (tail) {
            tail->next = t->next;
            if (tail->next == nullptr) {
                t->next = nullptr;
            } else {
                t->next = tail->next->next;
            }
            tail = tail->next;
            t = t->next;
        }
        
        return h;
    }
};
```