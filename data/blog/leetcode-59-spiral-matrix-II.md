---
title: Leetcode 59. Spiral Matrix II 解法筆記
date: '2025-04-24T00:37:00+08:00'
tags: ['LeetCode', 'Simulation']
draft: false
summary: 這題沒有運用到什麼DSA的技巧，只有存粹的模擬矩陣的運作
type: Blog
---
題目：
> Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.

Example：
```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```
## 解題思路
這題沒有什麼技巧，純粹就是考驗觀察的能力，還有邊界值的判斷。

因為這題需要建立四個邊界，讓for迴圈在跑的時候可以適時停下來，所以我們要先設定讓for跑到「離結束前的倒數第二個位置」。

為什麼是倒數第二個，不是第一個呢？這是因為如果跑到最後一個，那第二次跑的時候，指針就要從「第二個」元素開始跑，但是一旦跑到第四次的時候，指針會從「第二個」元素跑到「倒數第二個」元素，這樣會產生迴圈不統一的問題。

所以，我們統一規範「迴圈要從第一個跑到倒數第二個元素」。

有了這樣的先備條件後，寫起來就會比較容易！
## 實作
```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        // 設定上下左右的邊界值
        int boundary = n - 1;
        // 儲存Array跑到的位置
        int x = 0;
        int y = 0;
        //建立n * n的Array
        vector<vector<int>> ans(n, vector<int>(n));
        int num = 1;
        for(int k = 0 ; k < n / 2 ; k++) {
            for(int i = 0 ; i < boundary ; i++) {
                ans[x][y++] = num++;
            }
            for(int i = 0 ; i < boundary ; i++) {
                ans[x++][y] = num++;
            }
            for(int i = 0 ; i < boundary ; i++) {
                ans[x][y--] = num++;
            }
            for(int i = 0 ; i < boundary ; i++) {
                ans[x--][y] = num++;
            }
            x++;
            y++;
            boundary -= 2;
        }
        if(n % 2 == 1) {
            ans[x][y] = num;
        }
        return ans;
    }
};
```

## 總結
這題一開始解的時候是分別對上下左右建立邊界，但是發現這樣蠻麻煩的，所以這個版本改成統一用一個變數當作邊界，也比較好維護。

Array的題目基本上告一個段落，接下來會陸續更新Linked list的題目！