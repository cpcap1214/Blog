---
title: Leetcode 2. Add Two Numbers 解法筆記
date: '2025-05-09T17:00:00+08:00'
tags: ['LeetCode', 'Linked List']
draft: false
summary: 如果用暴力解，會因為數字太大而WA
type: Blog
---
題目：
> You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example：

![image](https://hackmd.io/_uploads/Sk3bEnrlgx.png)

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```
## 解題思路

很明顯的這題絕對不能把兩個數字直接加起來，因為題目有說最多有100個節點，也就是100位數，這樣連long long都不夠存。

這題的解決方式是直接把兩個List node的數字加起來，然後放到另一個Linked list裡面，如果超過10，就減10然後加1在下一個元素。

所以首先，建立兩個節點來同時遍歷兩個Linked list，並且同時把相加的元素放進去另一個Linked list，如果大於10就減10然後進位。

接著如果遇到一個Linked list已經走完了，就把剩下沒走完的另一個Linked list的全部元素加進去就好了。

## 實作

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* ans = new ListNode;
        ListNode* curr = ans;
        bool aboveTen = false;
        while(l1 && l2) { // 如果兩個Linked list都還沒走完
            if(aboveTen) { // 如果要進位
                curr->next = new ListNode(l1->val + l2->val + 1);
                aboveTen = false;
                if(curr->next->val >= 10) {
                    curr->next->val -= 10;
                    aboveTen = true;
                }
            }
            else {
                curr->next = new ListNode(l1->val + l2->val);
                if(curr->next->val >= 10) {
                    curr->next->val -= 10;
                    aboveTen = true;
                }
            }
            l1 = l1->next;
            l2 = l2->next;
            curr = curr->next;
        }
        while(!l1 && l2) { // 第二個Linked list還沒走完
            if(aboveTen) { // 如果要進位
                curr->next = new ListNode(l2->val + 1);
                aboveTen = false;
                if(curr->next->val >= 10) {
                    curr->next->val -= 10;
                    aboveTen = true;
                }
            }
            else {
                curr->next = new ListNode(l2->val);
                if(curr->next->val >= 10) {
                    curr->next->val -= 10;
                    aboveTen = true;
                }
            }
            l2 = l2->next;
            curr = curr->next;
        }
        while(l1 && !l2) { // 第一個Linked list還沒走完
            if(aboveTen) { // 如果要進位
                curr->next = new ListNode(l1->val + 1);
                aboveTen = false;
                if(curr->next->val >= 10) {
                    curr->next->val -= 10;
                    aboveTen = true;
                }
            }
            else {
                curr->next = new ListNode(l1->val);
                if(curr->next->val >= 10) {
                    curr->next->val -= 10;
                    aboveTen = true;
                }
            }
            l1 = l1->next;
            curr = curr->next;
        }
        if(aboveTen) { // 如果要進位，記得要補1
            curr->next = new ListNode(1);
        }
        return ans->next;
    }
};
```

## 總結

這題我的寫法比較長，因為我想要演示整個流程是怎麼運作的，當然也可以寫的很簡潔，像是把其中一個Linked list還沒有走完的部分跟兩個Linked list一起走的部分可以合併，但這樣也會比較難懂。
