title: LeetCode234. Palindrome Linked List
date: 2017-02-15 14:15:49
tags:
- 数据结构
- 算法
---

Given a singly linked list, determine if it is a palindrome.

Follow up:
Could you do it in O(n) time and O(1) space?

#### 把前一半反转一下，然后用两个指针，将一个移到中间后，开始逐个看两个指针上的数据是否相等

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
    bool isPalindrome(ListNode* head) {
        int len = getLength(head);
        if (len < 2)
            return true;
        ListNode *p1 = head, *p2 = head;
        bool result = true;
        int i = 0, mid = (len - 1) / 2;
        
        ListNode *reverseStart = head, *reverseEnd;
        while (p1 && p2) {
            if ((len % 2 == 0 && i == mid) || (len % 2 != 0 && i == mid - 1)) {
                reverseEnd = p2;
                p2 = p2->next;
                i++;
                p1 = reverse(reverseStart, reverseEnd);
            }
                
            if (i > mid) {
                if (p1->val != p2->val) {
                    result = false;
                    break;
                }
                p1 = p1->next;
                p2 = p2->next;
            } else {
                p2 = p2->next;
                i++;
            }
        }
        
        head = reverse(reverseEnd, reverseStart);
        return result;
    }
    
    ListNode* reverse(ListNode* start, ListNode* end) {
        if (start == end) {
            return start;
        }
        ListNode *cur = start, *next = cur->next, *tmp;
        while (cur != end) {
            tmp = next->next;
            next->next = cur;
            cur = next;
            next = tmp;
        }
        start->next = next;
        return cur;
    }
    
    int getLength(ListNode* head) {
        int len = 0;
        while (head) {
            len++;
            head = head->next;
        }
        return len;
    }
};
```
