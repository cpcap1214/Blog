---
title: Leetcode 24. Swap Nodes in Pairs 解法筆記
date: '2025-04-28T17:10:00+08:00'
tags: ['LeetCode', 'Linked List']
draft: false
summary: 先思考怎麼模擬交換Node，再開始實作會比較好
type: Blog
---
題目：

> Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

Example：

```md
Input: head = [1,2,3,4]

Output: [2,1,4,3]

Explanation:
```

![image](https://hackmd.io/_uploads/rkuUa2dJxx.png)

## 解題思路

這題跟[206. Reverse Linked List](https://hackmd.io/@YK1rWce-T264Hcp2UqDaZw/r1v5qOOkge)蠻像的，都是把現在的節點的next指向上一個。

跟206. 不太一樣的是要兩兩交換節點，所以我們就可以利用模擬的方式，走遍每一個節點，並且兩兩做交換。

比較麻煩的兩個節點交換的時候，要記錄第一個節點的上一個節點，這樣才能順利連接上去。

## 實作

### 左閉右閉

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(!head || head->next == nullptr) {
            return head;
        }
        ListNode* dummyHead = new ListNode; // 建立一個Dummy head
        ListNode* curr = dummyHead;
        dummyHead->next = head;
        while(curr->next && curr->next->next) { // 如果後兩個節點都存在
            ListNode* temp = curr->next->next; // 暫存第二個節點的位置
            curr->next->next = curr->next->next->next; // 第一個節點的next連到第三個節點
            temp->next = curr->next; // 第二個節點的next連到第一個節點
            curr->next = temp; // curr的下一個連到第二個節點
            curr = curr->next->next; // curr往後移兩個
        }
        return dummyHead->next;
    }
};
```

## 總結

這題我剛開始寫的時候，思路蠻複雜的，因為我開了很多個暫存的Node。

這個方法大幅簡化了記憶體空間。
