---
title: Leetcode 1. Two Sum 解法筆記
date: '2025-05-21T18:00:00+08:00'
tags: ['LeetCode', 'Hash Table']
draft: false
summary: 最有名的Two Sum終於來了
type: Blog
---
題目：
> Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.  
You may assume that each input would have exactly one solution, and you may not use the same element twice.  
You can return the answer in any order.

Example：
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```
## 解題思路

最經典的Two sum終於來了，當然這題用暴力解很簡單，但是時間複雜度就會到O(n^2)，有沒有什麼辦法可以把複雜度降低呢？

如果我們先把陣列裡面的元素都放到一個Hash table裡面，那我們就可以用O(n)的時間來找到我們要的元素在哪個位置了。

這一題我們會使用unordered_map這個容器，因為我們要同時記錄元素的值跟位置，所以要開一個pair來記錄這兩個資訊

## 實作

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> repr;
        for(int i = 0 ; i < nums.size() ; i++) {
            repr.insert(make_pair(nums[i], i)); // 把元素跟元素的位置放進去
        }
        vector<int> ans;
        for(int i = 0 ; i < nums.size() ; i++) {
            int val = target - nums[i]; // 計算要尋找的元素
            if(repr.count(val) && repr[val] != i) {
                ans.push_back(i);
                ans.push_back(repr[val]);
                break;
            }
        }
        return ans;
    }
};
```

## 總結

Hash table把兩個迴圈才能做到的事情，用一個迴圈就解決了，效率提升很多。

因為Hash table在尋找元素的時候，時間複雜度是O(1)，相比set的O(logn)來說會快很多。