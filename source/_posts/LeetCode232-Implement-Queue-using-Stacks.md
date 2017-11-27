title: LeetCode232. Implement Queue using Stacks
date: 2017-02-15 22:47:05
tags:
- 数据结构
- 算法
---

Implement the following operations of a queue using stacks.

push(x) -- Push element x to the back of queue.
pop() -- Removes the element from in front of queue.
peek() -- Get the front element.
empty() -- Return whether the queue is empty.
Notes:
You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

#### 用两个栈模拟，将数据始终塞进一个stack，pop时将数据弹出到另一个stack后再pop出来，就跟队列pop的顺序一样了

```
class MyQueue {
private:
    stack<int> s1, s2;
public:
    /** Initialize your data structure here. */
    MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if (s2.size() == 0) {
            while (s1.size() > 0) {
                s2.push(s1.top());
                s1.pop();
            }
        }
        
        int front = s2.top();
        s2.pop();
        return front;
    }
    
    /** Get the front element. */
    int peek() {
        if (s2.size() == 0) {
            while (s1.size() > 0) {
                s2.push(s1.top());
                s1.pop();
            }
        }
        return s2.top();
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return s1.size() == 0 && s2.size() == 0;
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * bool param_4 = obj.empty();
 */
```
