---
title: Leetcode 242. Valid Anagram 解法筆記
date: '2025-05-06T11:00:00+08:00'
tags: ['LeetCode', 'Hash Table']
draft: false
summary: 有限範圍的Hash table應用
type: Blog
---
題目：
> Given two strings s and t, return true if t is an anagram of s, and false otherwise.

Example：
```
Input: s = "anagram", t = "nagaram"
Output: true
```
## 解題思路

解這一題之前，我們要先定義什麼是Anagram。

Anagram的意思就是兩個單字如果可以用相同的英文字母組合而成，我們就叫這兩個單字互為Anagram。

所以我們這時候就可以用一個大小為26的Array，記錄每一個字母出現的次數，再比對另外一個單字可不可以恰好用這些數量的字母組合出來。


## 實作

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size() != t.size()) {
            return false;
        }
        int repr[26] = {0};
        for(int i = 0 ; i < s.size() ; i++) {
            repr[s[i] - 'a']++;
            repr[t[i] - 'a']--;
        }
        for(int i = 0 ; i < 26 ; i++) {
            if(repr[i] != 0) {
                return false;
            }
        }
        return true;
    }
};
```


## 總結

這題是很典型的「有限數量的Hash table」，因為題目已經規定字串裡面只會出現小寫字母，所以可以這樣寫。

但是如果遇到像是有關數字的題目，就會因為數字有無限多個，要另外開一個unorder_map或是set來儲存。

