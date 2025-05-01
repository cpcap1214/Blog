---
title: Leetcode 19. Remove Nth Node From End of List 解法筆記
date: '2025-05-01T10:00:00+08:00'
tags: ['LeetCode', 'Linked List']
draft: false
summary: 雖然暴力解跟優化解的時間複雜度一樣，但是執行時間還是有差
type: Blog
---
題目：
> Given the head of a linked list, remove the nth node from the end of the list and return its head.



Example：

![image](https://hackmd.io/_uploads/S1es9q9yee.png)


```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```
## 解題思路

這題最直覺的想法就是暴力解，先把整個Linked list跑過一遍後，記錄有多少個節點，最後在跑到倒數第N個節點就可以了。

這樣做有一個不好的地方：要開兩個迴圈，增加運行的時間。

所以這時候Two pointers就派得上用場了！只要開快慢指針，先讓快指針走N的節點，再讓快慢指針一起走，直到快指針到尾巴了，那這時候慢指針指向的Node，就是要被刪除的節點。

本題會用Dummy head的方式刪除節點，因為比較方便。

## 實作

### 暴力解
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(head == nullptr) {
            return nullptr;
        }
        if(head->next == nullptr) {
            return nullptr;
        }
        int size = 0;
        for(auto p = head ; p != nullptr ; p = p->next) {
            size++;
        }
        n = abs(n - size);
        if(n == 0) {
            return head->next;
        }
        ListNode* curr = head;
        while(n != 1) {
            curr = curr->next;
            n--;
        }
        ListNode* temp = curr->next;
        curr->next = temp->next;
        delete temp;
        return head;
    }
};
```
### Two pointers
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new dummyHead;
        dummyHead->next = head;
        ListNode* fast = dummyHead;
        ListNode* slow = dummyHead;
        for(int i = 0 ; i <= n ; i++) { // 讓快指針先跑過n + 1個Node
            fast = fast->next;
        }
        while(fast) {
            fast = fast->next;
            slow = slow->next;
        }
        ListNode* temp = slow->next;
        slow->next = temp->next;
        delete temp;
        return dummyHead->next;
    }
};
```

這邊可能很多人會有疑問，為什麼快指針先跑的時候，是要跑n + 1個節點，不是n個節點？

那是因為如果只跑了n個節點，那之後慢指針會指向「要被刪除的節點」的位置，但是Linked list要刪除節點的話，要知道「要被刪除的節點」的上一個的位置，才能刪除。

所以要讓slow少跑一個節點，才能把下一個節點（倒數第N個節點）刪除喔。


## 總結
這題雖然用暴力解跟Two pointers的時間複雜度都是O(n)，但是最壞的情況下暴力解會跑整整兩次，O(2n)的時間。

所以雖然O(2n) = O(n)，但是實際的時間會有差，尤其是資料量非常龐大的時候，Two pointers就會是最快的解法。