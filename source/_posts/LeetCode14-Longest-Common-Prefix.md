title: LeetCode14. Longest Common Prefix
date: 2017-02-13 23:56:23
tags: 
- 数据结构
- 算法 
---

Write a function to find the longest common prefix string amongst an array of strings.

#### 暴力穷举
先找出最短的一个字符串，然后遍历这个字符串的每个字符，与其余每个字符串对应的位置做比较，直到找到不同时中止，时间复杂度O(N*M)
```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size() == 0)
            return "";
        if (strs.size() == 1)
            return strs[0];
        
        int minlenstrpos = -1, minlen = 1000000;
        string result = "";
        for (int i = 0; i < strs.size(); i++) {
            if (strs[i].length() < minlen) {
                minlen = strs[i].length();
                minlenstrpos = i;
            }
        }
        
        int j = 0;
        for (; j < strs[minlenstrpos].length(); j++) {
            char cur = strs[minlenstrpos][j];
            for (auto str: strs) {
                if (str[j] == cur)
                    continue;
                else {
                    result = strs[minlenstrpos].substr(0, j);
                    j = strs[minlenstrpos].length() + 1;
                    break;
                }
            }
        }
        
        if (j == strs[minlenstrpos].length()) 
            result = strs[minlenstrpos];
        return result;
    }
};
```

#### 前缀树
时间复杂度O(N*M)
```
class Solution {
public:
    typedef struct NodeStruct {
        char c; // 记录节点字符
        int degree; // 记录节点的度 方便查找
        NodeStruct* child[26]; // 子节点
    } Node, *PtrToNode;
    
    PtrToNode createTrie() {
        PtrToNode root = (PtrToNode)malloc(sizeof(Node));
        root->degree = 0;
        for (int i = 0; i < 26; i++) 
            root->child[i] = NULL;
        return root;
    }
    
    void insertStrToTrie(PtrToNode root, const string str) {
        int len = str.length();
        if (len == 0)
            return;
        
        int i = 0, idx;
        PtrToNode head = root;
        for (; i < len; i++) {
            idx = str[i] - 'a';
            if (head->child[idx] != NULL) {
                head = head->child[idx];
            } else {
                PtrToNode newnode = (PtrToNode)malloc(sizeof(Node));
                newnode->c = str[i];
                newnode->degree = 0;
                for (int i = 0; i < 26; i++) 
                    newnode->child[i] = NULL;
            
                head->degree++;
                head->child[idx] = newnode;
                head = newnode;
            }
        }
    }
    
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size() == 0)
            return "";
        if (strs.size() == 1)
            return strs[0];
        
        PtrToNode root = createTrie();
        for (int i = 0; i < strs.size(); i++) 
            insertStrToTrie(root, strs[i]);
        
        int minlenstrpos = -1, minlen = 1000000;
        for (int i = 0; i < strs.size(); i++) {
            if (strs[i].length() < minlen) {
                minlen = strs[i].length();
                minlenstrpos = i;
            }
        }
        
        PtrToNode head = root; int j = 0;
        while (head->degree == 1 && j < strs[minlenstrpos].length()) {
            head = head->child[strs[minlenstrpos][j++] - 'a'];
        }
        
        return strs[0].substr(0, j);    
    }
};
```
