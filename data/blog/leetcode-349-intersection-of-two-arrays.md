---
title: Leetcode 349. Intersection of Two Arrays 解法筆記
date: '2025-05-08T23:00:00+08:00'
tags: ['LeetCode', 'Hash Table']
draft: false
summary: 開始運用到C++內建的容器了，而不再只是用Array當作Hash talbe
type: Blog
---
題目：
> Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.

Example：
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```
## 解題思路

終於遇到不能只開有限長度的Array了，因為這一題的數字是整數（雖然題目還是有限制整數的範圍），所以要開始用其他容器來裝了。

在實作Hash Table的時候，大致而言是用這三種容器：

1. Set
2. Unordered_set
3. Unordered_map

其中Set還可以分成一般的Set跟Multiset，Multiset簡單來說就是可以存放重複的元素，這邊就不多做介紹。

這題的答案由於是輸出不重複的元素，而且也不用排序，所以這題會選用Unordered_set，來將重複的元素給踢掉。


## 實作

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> ans;
        unordered_set<int> repr1;
        // 先把第一個vector的元素放到unordered_set裡面
        unordered_set<int> repr2(nums1.begin(), num1.end()); 
        for(int i = 0 ; i < nums2.size() ; i++) {
            // 如果第二個vector也有出現第一個vector的元素，加進另一個unordered_set
            if(repr2.count(nums2[i])) {
                repr1.insert(nums2[i]);
            }
        }
        for(auto val : repr1) {
            ans.push_back(val);
        }
        return ans;
    }
};
```

## 總結

雖然基本上全部的Hash table題目都可以用set來實作，但是set所需的空間遠比Array還要大，如果遇到有限元素題目，用Array就夠用了，不用特地開到set。