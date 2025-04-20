---
title: Leetcode 704. Binary Search 解法筆記
date: 2025-04-20
tags: ['LeetCode', 'Binary Search']
draft: false
summary: 這題是典型Binary search的題目，重點是要考慮區間的取值。
type: Blog
---
題目：
> Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

Example：
```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```
## 解題思路
這題看到題目說已經排好序，然後又不重複，就大概知道這題是要用Binary search，而且是用最基本的那種（雖然我也不會其他種）。
總之，直接implement就對了。
## 實作
在寫Binary search的時候，會考慮左界跟右界的區間範圍。
大致上可以分成：
1. 左閉右閉
2. 左閉右開

區間範圍會跟左右界怎麼更新有關，所以要固定住一種區間。
我一開始是用左閉右閉，但下面我兩種做法都會寫。
### 左閉右閉
```cpp=
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        // 因為是左閉右閉，所以最右邊的值會考慮到，所以要把右界直接變成最後面的元素的位置
        int right = nums.size() - 1;
        while(left <= right) { // 左閉右閉，所以要加上等於
            int mid = left + (right - left) / 2;
            if(nums[mid] > target) {
                right = mid - 1; // -1是因為這是左閉右閉的區間，
                                 // 所以剛剛已經試過右邊不行，就不用再試一次了。
            }
            else if(nums[mid] < target) {
                left = mid + 1;
            }
            else {
                return mid;
            }
        }
        return -1;
    }
};
```
### 左閉右開
```cpp=
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        // 因為是左閉右開，所以最右邊的值不會考慮到，所以要把右界變成這個陣列的size
        int right = nums.size();
        while(left < right) { // 左閉右開，所以不用考慮到最右邊
            int mid = left + (right - left) / 2;
            if(nums[mid] > target) {
                right = mid; // 不用-1，因為在上一次的判斷中，沒有判斷最右邊的值
            }
            else if(nums[mid] < target) {
                left = mid + 1;
            }
            else {
                return mid;
            }
        }
        return -1;
    }
};
```

## 總結
這是我第一題的Leetcode，也是我第一次打這種筆記文章。

打這種筆記文章的目的是為了幫我複習我這道題是怎麼解的，也有點像是費曼學習法一樣，「如果想要學會這門學問，就要想辦法教會別人。」
總之，希望可以繼續寫下去！