title: LeeCode383. Ransom Note
date: 2017-02-19 20:41:59
tags:
- 数据结构
- 算法
---

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

Note:
You may assume that both strings contain only lowercase letters.
```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

```
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        bool result = true;
        unordered_map<char, int> m;
        for (int i = 0, len = magazine.length(); i < len; i++) {
            m[magazine[i]]++;
        }
        for (int j = 0, len = ransomNote.length(); j < len; j++) {
            if (m[ransomNote[j]]-- == 0) {
                result = false;
                break;
            }
        }
        return result;
    }
};
```
