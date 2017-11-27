title: LeetCode141. Linked List Cycle
date: 2017-02-15 00:29:00
tags:
- 数据结构
- 算法
---

Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

#### 一个笨办法是将每个节点插入一个unordered_map里，每次插入前去查一下是否已经存在，这个方法需要额外的存储空间；另一个方法是用两个指针遍历链表，一个指针每次走一步，另一个指针每次走两步，如果有环，这两个指针必定会相遇，具体代码如下

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
    bool hasCycle(ListNode *head) {
        if (head == nullptr) 
            return false;
        ListNode *p1 = head, *p2 = head;
        bool result = false;
        while (p1 && p2) {
            p1 = p1->next;
            p2 = p2->next;
            if (!p2) {
                result = false;
                break;
            } else {
                p2 = p2->next;
            }
            
            if (p1 == p2) {
                result = true;
                break;
            }
        }
        return result;
    }
};
```