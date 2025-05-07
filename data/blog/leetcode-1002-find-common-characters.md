---
title: Leetcode 1002. Find Common Characters 解法筆記
date: '2025-05-07T16:00:00+08:00'
tags: ['LeetCode', 'Hash Table']
draft: false
summary: 開始進入到多個Hash table的比較
type: Blog
---
題目：
> Given a string array words, return an array of all characters that show up in all strings within the words (including duplicates). You may return the answer in any order.

Example：
```
Input: words = ["bella","label","roller"]
Output: ["e","l","l"]
```
## 解題思路

這題的字母有限定只有小寫字母，所以一樣是用長度為26的Array儲存一個單字有出現多少次字母。

比較不同的是不能像上一題一樣，直接儲存每一個字母出現多少次，然後取平均，因為有可能會有一個字母在同一個單字很常出現，但是在另一個單字卻完全沒有出現。

由於這題要求每個單字都有出現的，所以在跟每一個單字比較時，我們要把出現的次數取較小的值，才不會出問題。

具體來說，每一個單字都可以表示成長度為26的Array，每一個位置都代表第N的字母。每一次比完一個單字時，都要把兩個Array的值取min，才可以確保這些字母在每一個單字都有出現。

## 實作

```cpp
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        int repr[26] = {0};
        // 先幫第一個單字建立Hash table
        for(int i = 0 ; i < words[0].size() ; i++) {
            repr[words[0][i] - 'a']++;
        }
        // 對每一個單字建立Hash table，再跟原始的Hash table比較
        for(int i = 1 ; i < words.size() ; i++) {
            int temp[26] = {0};
            for(auto c : words[i]) {
                temp[c - 'a']++;
            }
            for(int i = 0 ; i < 26 ; i++) {
                repr[i] = min(repr[i], temp[i]); // 取小的值
            }
        }
        vector<string> ans;
        for(int i = 0 ; i < 26 ; i++) {
            while(repr[i] > 0) {
                repr[i]--;
                string c{static_cast<char>('a' + i)};
                ans.push_back(c);
            }
        }
        return ans;
    }
};
```

## 總結

基本上看到有要比較重複出現的元素，都可以用Hash table來記錄、處理，這樣一定會比暴力解還要快喔。

當然這是限定在「元素是在有限的範圍」之內才能這樣，像是這一題只會有26種元素要記錄，但是後面的題目就會遇到元素是整數的情況，這樣就不只是開一個Array這麼簡單！
