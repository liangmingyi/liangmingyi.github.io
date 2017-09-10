title: LeetCode28. Implement strStr()
date: 2017-03-12 12:28:40
tags:
- 数据结构
- 算法
---

Implement strStr().

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.


简单的做法双重循环，依次去比较，时间复杂度O(m*n)，也可以用KMP

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        int len1 = haystack.length(), len2 = needle.length();
        if (len2 > len1) return -1;
        if (len1 == 0 && len2 == 0) return 0;

        for (int i = 0; i < len1 - len2 + 1; i++) {
            int j = 0;
            for (; j < len2; j++) {
                if (haystack[i + j] != needle[j])
                    break;
            }
            if (j == len2)
                return i;
        }
        return -1;
    }
};
```

翻出算法导论，复习了下KMP，无论理解还是写起来都还是比较费劲的（只见过一位清华学霸徒手写完整过），网上的解读资料也是非常多的，有一篇写的比较好的，[KMP算法详解](http://www.matrix67.com/blog/archives/115)  最优时间复杂度O(n)
```
class Solution {
public:
    vector<int> failure;

    void GetFailureFunction(string& needle){

        failure.assign(needle.size(), -1);

        for(int j = 1; j < needle.size(); j++){
            int i = failure[j-1];

            while( (needle[j] != needle[i+1]) && (i>=0) ){
                i = failure[i];
            }

            if(needle[j] == needle[i+1]){
                failure[j] = i+1;
            }
        }
    }

    int KMP(string& haystack, string& needle){
        int i = 0, j = 0;
        while( i<haystack.size() && j<needle.size() ){
            if( haystack[i] == needle[j] ){
                i++; j++;
            }
            else{
                if( j == 0 )
                    i++;
                else
                    j = failure[j-1] + 1;
            }
        }

        if(j < needle.size())
            return -1;
        else
            return i-needle.size();
    }

    int strStr(string haystack, string needle) {

        GetFailureFunction(needle);

        return KMP(haystack,needle);
    }
};
```
