---
title: Leetcode 977. Squares of a Sorted Array 解法筆記
date: '2025-04-21T16:00:00+08:00'
tags: ['LeetCode', 'Two Pointers']
draft: false
summary: 如果不用Two pointers的話，複雜度會到O(nlogn)！
type: Blog
---
題目：
> Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

Example：
```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```
## 解題思路
這題的第一個反應是先把整個Array的元素平方後，再sort一遍就好。但畢竟sort至少要O(nlogn)，所以還要再想想看有什麼更好的方法。

可以觀察後發現，因為這個Array其實已經sort好了，所以平方後最大的值一定會在最左邊或最右邊。這樣就可以透過Two pointers來從兩端篩選資料。

具體的方式大概是：
1. 先全部平方。
2. 建立兩端的指針，分別指向第一個元素跟最後一個元素。
3. 比較哪一個元素比較大，之後把比較大的放到另一個Array。
4. 比較大的那端的指針往前進（往後退）一格。
5. 重複步驟三跟步驟四，直到整個Array都跑過一次。


## 實作

### 暴力解
```cpp=
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> temp(nums.begin(), nums.end());
        for(int i = 0 ; i < nums.size() ; i++) {
            temp[i] *= temp[i];
        }
        sort(temp.begin(), temp.end());
        // 反轉整個Array，因為題目要求由大到小
        reverse(temp.rend(), temp.rbegin()); 
        return temp;
    }
};
```
### Two pointers
```cpp=
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> temp(nums.begin(), nums.end());
        for(int i = 0 ; i < nums.size() ; i++) {
            temp[i] *= temp[i];
        }
        vector<int> ans(nums.size());
        int left = 0;
        int right = nums.size() - 1;
        for(int i = nums.size() - 1 ; i >= 0 ; i--) {
            if(temp[left] < temp[right]) {
                ans[i] = temp[right];
                right--;
            }
            else {
                ans[i] = temp[left];
                left++;
            }
        }
        return ans;
    }
};
```

## 總結
這題就是很典型的Two pointers的應用。

我覺得自從我開始寫LeetCode的時候，不會在第一時間就想到要用暴力解，而是會開始想說用哪種方式可以降低複雜度，感覺這樣蠻不錯的。