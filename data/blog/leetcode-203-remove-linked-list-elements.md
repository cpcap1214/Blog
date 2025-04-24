---
title: Leetcode 203. Remove Linked List Elements 解法筆記
date: '2025-04-24T17:00:00+08:00'
tags: ['LeetCode', 'Linked List']
draft: false
summary: 進入Linked list後，大致會分成兩種做法：Dummy head或是頭節點分開處理
type: Blog
---
題目：
> Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

Example：
![image](https://hackmd.io/_uploads/BJSnRI8kge.png)
```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```
## 解題思路

相信大家已經對Linked list不陌生了，如果不太清楚，我之後會在寫有關資料結構的筆記。

總之，通常我們要對Linked list做操作的時候，會用兩種方式：虛擬節點頭（Dummy head）或是頭跟其他部分分開處理。

會分成兩種方式是因為如果要進行新增、刪除或其他操作時，一定要有上一個Node的存在，才能操作。可以想想看如果要刪除一個Node的話，如果沒有上一個Node，就不知道要怎麼把上一個跟下一個Node串在一起，所以才會用其他方式解決。

基本上在寫Linked list的題目時，會傾向用Dummy head的方式，因為如果把頭跟其他部分分開處理的話，會要在同一個Function中使用兩種不同的邏輯，這樣會讓程式變的更複雜。

但是用Dummy head的話，就會把用同一個方法對Linked list進行操作，比較直觀，也比較容易除錯。

回到解題思路，其實這題只要判斷該節點的Value是不是要被刪除的Value，如果是的話，就把上一個節點的next接到要刪除的節點的下一個。

## 實作

實作之前，先放上Leetcode對List node的定義：
```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```
簡單來說就是每個Node當中會有一個整數的值（val），還有一個指向下一個Node的值（next）。

### Dummy head
```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if(head == nullptr) {
            return head;
        }
        ListNode* dummyHead = new ListNode; // 建立虛擬頭節點
        dummyHead->next = head; // 虛擬頭節點的下一個Node指向head
        // 走過每一個節點，並且判斷每一個節點的下一個的值是不是val
        for(auto p = dummyHead ; p != nullptr ; p = p->next) {
            while(p->next && p->next->val == val) {
                ListNode* temp = p->next; // 暫時存放待刪除的節點的位置
                p->next = temp->next;
                delete temp; // 刪除該節點
            }
        }
        return dummyHead->next;
    }
};
```
### 頭節點分開操作
```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if(head == nullptr) {
            return head;
        }
        while(head && head->val == val) { // 處理head的部分
            ListNode* temp = head;
            head = head->next;
            delete temp;
        }
        // 處理不是head的部分
        for(auto p = head ; p != nullptr ; p = p->next) {
            while(p->next && p->next->val == val) {
                ListNode* temp = p->next;
                p->next = temp->next;
                delete temp;
            }
        }
        return head;
    }
};
```

## 總結
記得剛開始學到Linked list的時候很困惑，覺得為什麼要搞得這麼複雜，結果現在寫了一些後，才發現Linked list可以做到真正刪除一個記憶體的空間，而不是像Array那樣用覆蓋的方式刪除元素。真的很神奇！