---
title: Leetcode 206. Reverse Linked List 解法筆記
date: '2025-04-27T17:00:00+08:00'
tags: ['LeetCode', 'Linked List']
draft: false
summary: 道理很容易懂，但是要用理解的，不要直接把做法背下來
type: Blog
---
題目：

> Given the head of a singly linked list, reverse the list, and return the reversed list.

Example：
![image](https://hackmd.io/_uploads/Sy0T9u_1gx.png)

```md
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

## 解題思路

這題也是Linked list中很常出現的題目，雖然很基礎，可是很多人都會因為不懂原理，所以直接把解法背下來，這樣就沒有真的去理解怎麼reverse的。

想要reverse一個Linked list，最重要的關鍵就是把「Node的next指向上一個Node」，為此，我們會需要先建立兩個Node：

1. 指向前一個節點的Node
2. 記錄現在節點的Node

每一次要進行reverse的時候，會先把現在節點的Next指到前一個節點，再繼續處理下一個Node，直到全部都處理完畢。

## 實作

### ListNode的定義

```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```

### 程式碼

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr; // 記錄上一個節點的位置，初始為nullptr
        ListNode* curr = head; // 記錄當前節點的位置
        while(curr != nullptr) { // 如果curr還沒有走完整個Linked list，就要繼續處理
            ListNode* temp = curr->next; // 建立一個暫時的節點，記錄下一
                                         // 個Node的位置
            curr->next = prev; // 現在節點的next指向上一個
            prev = curr; // 記錄上一個節點的Node變成現在的節點
            curr = temp; // 走到下一個節點，繼續reverse
        }
        return prev; // 回傳最後一個節點（現在變第一個）的位置
    }
};
```

## 總結

記得之前看過一個梗圖，就是有一個很會做Side projects的人去面試，面試官就說：「你很會做專案沒錯，但你會reverse linked list嗎？」結果他不會，就被淘汰了。

雖然這題很基本，但是就跟Binary search一樣，要用理解的方式學會這一題，而不是背下來。
