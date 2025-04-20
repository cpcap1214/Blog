---
title: Leetcode 27. Remove Element 解法筆記
date: 2025-04-21
tags: ['LeetCode', 'Two Pointers']
draft: false
summary: 這題是很經典的Two pointers的應用
type: Blog
---
題目：
> Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Example：

```md
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements 
of nums being 2.
It does not matter what you leave beyond the returned k (hence they are
underscores).
```

## 解題思路

這題可以用暴力解，就是把每一個要被刪除的元素，都用後面的補上去，當然這樣複雜度就會變成O(n^2)。

另一種方式就是直接用vector的erase，但是這樣就沒有什麼意義了。

我覺得如果有一個題目可以直接使用內建的function來解決的話，那反而不應該使用那些function，要不然這題就沒有什麼意義了。

但是如果有一個function只是解決這題的其中一個步驟，像是sort的話，就應該要使用這些function。

回歸正題，如果想要把複雜度壓到O(n)的話，就要用Two pointers的做法。

### 什麼是Two pointers？

Two pointers簡單來說就是利用兩個指針，把需要用兩層for迴圈執行的步驟，簡化到一個for迴圈就可以做了。

我會在這題建立一個快指針，一個慢指針。快指針是用來掃過陣列裡面每一個元素，而慢指針是用來把不要的元素覆蓋，也就是更新陣列。

針對每一個快指針掃到的元素，如果這個元素不等於我們要刪除的數字，我們就會讓慢指針所在的位置用快指針的元素取代，並且慢指針往前移一格。

但是如果這個元素是我們要刪除的數字，我們就不會讓慢指針移動，而是讓快指針繼續往前。

因為我們不需要保留需要刪除的元素，所以只要讓慢指針待在原地就好。

## 實作

### 暴力解

```cpp=
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int count = nums.size();
        for(int i = 0 ; i < nums.size() ; i++) {
            if(nums[i] == val) {
                for(int j = i + 1 ; j < nums.size() ; j++) {
                    nums[j - 1] = nums[j];
                }
                i--;
                count--;
            }
        }
        return count;
    }
};
```
這個會TLE，所以基本上不能這樣做。

### Two pointers

```cpp=
class Solution {
public:
    int removeElement(vector<int>& nums, int target) {
        int slow = 0;
        for(int fast = 0 ; fast < nums.size() ; fast++) {
            if(nums[fast] != target) {
                nums[slow++] = nums[fast]; // 把slow所在位置的元素換成fast的元素
            }
        }
        return slow;
    }
};
```

### function解
```cpp=
class Solution {
public:
    int removeElement(vector<int>& nums, int target) {
        for(int i = 0 ; i < nums.size() ; i++) {
            if(nums[i] == target) {
                nums.erase(nums.begin() + i);
                i--;
            }
        }
        return nums.size();
    }
};
```
## 總結

這題算是我第一次碰到Two pointers的題目，覺得可以簡化很多時間，蠻酷的。之後會提到的Sliding window也是基於Two pointers的基礎出發的。

