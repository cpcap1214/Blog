---
title: Leetcode 160. Intersection of Two Linked Lists 解法筆記
date: '2025-05-05T00:00:00+08:00'
tags: ['LeetCode', 'Linked List']
draft: false
summary: Two pointers的應用之一，比暴力解快很多
type: Blog
---
題目：
> Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.
For example, the following two linked lists begin to intersect at node c1:
![image](https://hackmd.io/_uploads/SkW5jIn1ee.png)
The test cases are generated such that there are no cycles anywhere in the entire linked structure.
Note that the linked lists must retain their original structure after the function returns.

Example：
```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], 
skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if 
the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as 
[5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; 
There are 3 nodes before the intersected node in B.
- Note that the intersected node's value is not 1 because the nodes with value 1 
in A and B (2nd node in A and 3rd node in B) are different node references. 
In other words, they point to two different locations in memory, while the nodes 
with value 8 in A and B (3rd node in A and 4th node in B) point to the same 
location in memory.
```
## 解題思路

這題最直覺的方式就是暴力解，走遍第一個Linked list的每個節點，再依依比對是不是跟第二個節點有一樣的記憶體位置，如果有，就回傳這個節點。

很明顯這樣的解法會是O(n * m)，那有沒有更快的方式，或是說，不用一個一個比對呢？

解決方法也很簡單：讓兩個Linked list的起始點一樣，就可以一次比較兩個節點的位置了。

具體來說，先求出兩個Linked list的長度，如果長度一樣，代表我們只要開一個for迴圈，就可以走過兩個Linked list的每一個節點。

如果長度不一樣，只要先讓比較長的Linked list走「兩個長度相差」的節點數量，就可以讓起始位置一樣了，一樣只要開一個for迴圈就可以。

其實很多題目都是「想辦法把需要n個迴圈的暴力解，化減成只要用n - 1個迴圈」。只要根據這個核心思想，就可以把複雜度降低。

像是這題如果只開一個for迴圈的話，就可以把時間複雜度降到O(n + m)，遠比暴力解還要快。

## 實作

### 暴力解
```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* first = headA;
        ListNode* second = headB;
        for( ; first != nullptr ; first = first->next) {
            for(second = headB ; second != nullptr ; second = second->next) {
                if(first == second) {
                    return first;
                }
            }
        }
        return nullptr;
    }
};
```
### 求長度後再同時比對Node
```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* first = headA;
        ListNode* second = headB;
        int firstLength = 0;
        int secondLength = 0;
        while(first) { // 求第一個Linked list的長度
            firstLength++;
            first = first->next;
        }
        while(second) { // 求第二個Linked list的長度
            secondLength++;
            second = second->next;
        }
        if(firstLength > secondLength) { // 如果第一個Linked list比較長
                                         // 就先讓第一個走兩個長度差的Node
            for(int i = 0 ; i < firstLength - secondLength ; i++) {
                headA = headA->next;
            }
            while(headA && headB) { // 一個一個Node比對
                if(headA == headB) {
                    return headA;
                }
                headA = headA->next;
                headB = headB->next;
            }
            return nullptr;
        }
        else { // 如果第二個Linked list比較長，就先讓第二個走兩個長度差的Node
            for(int i = 0 ; i < secondLength - firstLength ; i++) {
                headB = headB->next;
            }
            while(headA && headB) { // 一個一個Node比對
                if(headA == headB) {
                    return headA;
                }
                headA = headA->next;
                headB = headB->next;
            }
            return nullptr;
        }
    }
};
```

## 總結

這題跟Two pointers的理念很像：「用一個for迴圈做兩個for迴圈的事情」，只是我們先處理長度的部分，再讓兩個指針根據長度大小調整位置，最後用for迴圈一個一個比對Node是不是一樣的。

未來還會遇到很多用Two pointers的問題，只要用前面提到的方式去判斷就可以了。
