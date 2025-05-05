---
title: Leetcode 142. Linked List Cycle II 解法筆記
date: '2025-05-05T12:00:00+08:00'
tags: ['LeetCode', 'Linked List']
draft: false
summary: 一樣是雙指針結合Linked list的應用
type: Blog
---
題目：
> Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.
There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.
Do not modify the linked list.

Example：

![image](https://hackmd.io/_uploads/r1vhGGp1ll.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to
the second node.
```
## 解題思路

這題我一開始的做法是用Hash table的方式，把每一個走過的Node都存進去unorder_set裡面，然後每次新增Node前都會檢查Node是不是有重複了，如果重複就return那個節點的index。

雖然這樣時間複雜度會是O(n)，但是還要再多開一個記憶體空間儲存Node的資訊，這樣空間複雜度會變成O(n)，有沒有什麼方式可以不用多開unordered_set，讓空間複雜度降到O(1)呢？

這時候我們一樣可以使用雙指針來運作，利用快慢指針會不會相遇，來判斷這個Linked list有沒有環。

為什麼快慢指針可以用來檢測有沒有環呢？我們先讓快指針每次會走過兩個Node，慢指針每次會走一個Node，如果兩個指針都進入環裡面了，我們可以認定，快指針其實是在以每次走過一個Node的速度在追慢指針。

因為在環裡面，可以想像慢指針看向快指針的時候，雖然慢指針每次都會往前一個Node，但是快指針也會往前兩個，所以實際上快指針一定可以追的到慢指針。

只要追上，就代表這個Linked list有環，如果快指針變成nullptr，就代表沒有環。

判斷是否有環的部分處理完了，接下來要處理環是從哪裡開始。

其實用數學的方式就可以推導出來，環會在「快指針和頭節點同時走，相遇的地方」。我這邊就不做推導了，有興趣的人可以自己推推看，或是去看這一題的題解。

總之，大致了解思路後，就可以開始實作了！

## 實作

### Hash table
```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode*> repr;
        while(head) {
            if(!repr.count(head)) { // 如果這個Node沒有走過，就加到Hash table
                repr.insert(head);
                head = head->next;
            }
            else { // 有出現過，代表有環，直接回傳這個節點的位置
                return head;
            }
        }
        return nullptr;
    }
};
```
### Two pointers
```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast && fast->next) {
            fast = fast->next->next; // 快指針走兩格
            slow = slow->next; // 慢指針走一格
            if(fast == slow) { // 如果快指針跟慢指針相遇，代表有環
                while(fast != head) {
                    fast = fast->next;
                    head = head->next;
                }
                return head;
            }
        }
        return nullptr;
    }
};
```

## 總結
Linked list到這邊告一段落，雖然之後還有一點題目，但是準備要進到Hash table了。

這一題開始有碰到Hash table，不過用set也可以處理這一題，如果不太了解什麼是Hash table的話，下一題會比較完整的解釋觀念以及用法！