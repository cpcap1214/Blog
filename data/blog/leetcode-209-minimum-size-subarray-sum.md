---
title: Leetcode 209. Minimum Size Subarray Sum 解法筆記
date: '2025-04-21T17:00:00+08:00'
tags: ['LeetCode', 'Two Pointers', 'Sliding Window']
draft: false
summary: Two pointers的延伸應用
type: Blog
---
題目：
> Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

Example：
```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```
## 解題思路
開始解這題之前，有一個很重要的觀念要澄清：
> Subarray是代表這個Array中，**連續**的子陣列。

當初在解這題的時候，完全想不到要怎麼解，因為我以為Subarray不一定要連續，所以完全沒有想過可以用兩個指針慢慢掃。

有別於Two pointers，這次會用另一種技巧：Sliding window。

### 什麼是Sliding window？
Sliding window跟Two pointers很類似，都是用兩個指針去掃過特定的範圍。但是Sliding window跟Two pointers最大的不同就是「元素的搜集範圍」。

基本上Two pointers只會對兩個指針指導的那個特定元素做處理，但是Sliding window是搜集兩個指針的範圍內的元素，再進行額外的處理。

比方這一題就是要我們求Subarray內的總和，所以這題的Sliding window的範圍內就會用相加的方式來計算Sum。

簡單來說，其實Sliding window就是Two pointers的延伸應用！

## 實作

### 左閉右閉
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int sum = 0;
        int minArrayLength = INT_MAX;
        for(int i = 0 ; i < nums.size() ; i++) {
            sum = 0;
            for(int j = i ; j < nums.size() ; j++) {
                sum += nums[j];
                if(sum >= target) {
                    minArrayLength = min(minArrayLength, j - i + 1);
                    break;
                }
            }
        }
        return minArrayLength == INT_MAX ? 0 : minArrayLength;
    }
};
```

這樣時間複雜度是O(n^2)，會TLE，所以不能這樣做
### Sliding window
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int sum = 0;
        int minArrayLength = INT_MAX;
        int slow = 0; // 建立慢指針，用來紀錄Subarray最尾端的位置
        for(int fast = 0 ; fast < nums.size() ; fast++) {
            sum += nums[fast];
            while(sum >= target) { // 這邊用while去判斷，
                                   // 才會考慮到慢指針要移動多次的情況。
                minArrayLength = min(minArrayLength, fast - slow + 1);
                sum -= nums[slow];
                slow++;
            }
        }
        // 如果沒有Subarray的總和符合target，回傳0。
        return minArrayLength == INT_MAX ? 0 : minArrayLength;
    }
};
```

## 總結
有關Array的題目差不多結束了，下一題會比較偏向模擬的題目，也沒有什麼技巧，就是要想清楚就對了！