---
title: Leetcode 202. Happy Number 解法筆記
date: '2025-05-12T18:00:00+08:00'
tags: ['LeetCode', 'Hash Table']
draft: false
summary: unordered_set的經典應用
type: Blog
---
題目：
> Write an algorithm to determine if a number n is happy.
A happy number is a number defined by the following process:
> - Starting with any positive integer, replace the number by the sum of the squares of its digits.
> - Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
> - Those numbers for which this process ends in 1 are happy.
> 
> Return true if n is a happy number, and false if not.


Example：
```
Input: n = 19
Output: true
Explanation:
1 ** 2 + 9 ** 2 = 82
8 ** 2 + 2 ** 2 = 68
6 ** 2 + 8 ** 2 = 100
1 ** 2 + 0 ** 2 + 0 ** 2 = 1
```
## 解題思路

這題最重要的是怎麼求出「找出快樂數的計算出現循環」，總不可能用for迴圈循環個50次後發現還沒有變成1，就直接輸出「不是快樂數」吧。

如果已經變成循環的話，會發現循環是因為出現重複的數字，比方2這個數字，會變成下面這樣：

- 2
- 4
- 16
- 37(1 ** 2 + 6 ** 2)
- 58(3 ** 2 + 7 ** 2)
- 89(5 ** 2 + 8 ** 2)
- 145
- 42
- 20
- 4

因為4已經出現過了，所以我們知道進入循環裡面了。

也就是說，如果出現重複的數字，我們就知道進到循環，反之如果出現1，就代表這是快樂數。

所以我們會開一個unordered_set來儲存每一次的數字，並且每一次都找這個數字有沒有出現過。

## 實作

```cpp
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> repr;
        int sum = 0;
        while(true) { // 用無窮迴圈，運行直到變成0或是進入循環
            while(n > 0) {
                int temp = n % 10;
                sum += temp * temp;
                n /= 10;
            }
            if(sum == 1) {
                return true;
            }
            if(repr.count(sum)) {
                return false;
            }
            else {
                repr.insert(sum);
                n = sum;
                sum = 0;
            }
        }
    }
};
```


## 總結

這題我一開始的確是用for迴圈跑50次，然後就過了！蠻好笑的

但是上面的做法才是正確的喔，而且因為unordered_set的底層容器是用Hash table，所以會比用紅黑樹實作的set還要快！