---
title: Leetcode 707. Design Linked List 解法筆記
date: '2025-04-26T00:00:00+08:00'
tags: ['LeetCode', 'Linked List']
draft: false
summary: 這題把Linked list最常用的function都寫了一遍，很實用的一題
type: Blog
---
題目（直接截LeetCode上面的）：

![CleanShot 2025-04-24 at 12.44.01@2x](https://hackmd.io/_uploads/B15i-SvJge.png)

Example：

```md
Input
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", 
"deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
Output
[null, null, null, null, 2, null, 3]

Explanation
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
myLinkedList.get(1);              // return 2
myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
myLinkedList.get(1);              // return 3
```

## 解題思路

簡單來說，這題要我們實作Linked list的五個主要功能：

1. Constructor：建構一個MyLinkedList()
2. get(index)：讀取第index的值，如果index不合理（超出範圍、負數），則回傳-1
3. addAtHead(val)：在頭部插入一個值為val的Node
4. addAtTail(val)：在尾部插入一個值為val的Node
5. addAtIndex(index, val)：在第index的Node前差入一個值為val的Node，如果index等於這個Linked list的長度，就在最後面插入。
6. deleteAtIndex(index)：刪除最第index的Node。

至於我們要用Singly或是Doubly的Linked list，以及Node的class要怎麼寫，都交給我們自己定義，只要function名稱一樣就好了。

這次的解題筆記會以Singly linked list為主，並且都用「頭節點分開操作」的方式操作整個Linked list。

## 實作

在這次的筆記，我會把每一個function分開寫，方便閱讀，但是提交到LeetCode的時候要合在一起喔。

### 建構ListNode

```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int entry) {
        val = entry;
        next = nullptr;
    }
};
```

### MyLinkedList的constructor

```cpp
class MyLinkedList {
private:
    ListNode* head;
    int size; // 用來記錄Linked list的大小
public:
    MyLinkedList() {
        head = nullptr;
        size = 0;
    }
};
```

### get()

```cpp
int get(int index) {
    if(index < 0 || index >= size) { // 如果不符合index的規則，就直接return -1
        return -1;
    }
    ListNode* p = head;
    for(int i = 0 ; i < index ; i++) { // 用for迴圈走到index的位置
        p = p->next;
    }
    return p->val;
}
```

### addAtHead()

```cpp
void addAtHead(int val) {
    ListNode* tempHead = new ListNode(val);
    tempHead->next = head;
    head = tempHead;
    size++;
}
```

### addAtTail()

```cpp
void addAtTail(int val) {
    if(size == 0) { // 如果沒有節點的話，就直接用head新增節點
        head = new ListNode(val);
        size++;
        return;
    }
    ListNode* p = head;
    for(int i = 0 ; i < size - 1 ; i++) {
        p = p->next;
    }
    ListNode* tail = p->next;
    p->next = new ListNode(val);
    p->next->next = tail;
    size++;
}
```

### addAtIndex()

```cpp
void addAtIndex(int index, int val) {
    if(index < 0 || index > size) {
        return;
    }
    if(index == 0) { // 如果index是0，可以直接呼叫addAtHead()，節省時間
        addAtHead(val);
        return;
    }
    ListNode* p = head;
    for(int i = 0 ; i < index - 1 ; i++) {
        p = p->next;
    }
    ListNode* temp = p->next;
    p->next = new ListNode(val);
    p->next->next = temp;
    size++;
}
```

### deleteAtIndex()

```cpp
void deleteAtIndex(int index) {
    if(size == 0) {
        return;
    }
    if(index < 0 || index >= size) {
        return;
    }
    if(size == 1) { // 如果只有一個節點，直接刪除head後，把head設為nullptr
        delete head;
        head = nullptr;
        size--;
        return;
    }
    if(index == 0) {
        ListNode* temp = head;
        head = head->next;
        delete temp;
        size--;
        return;
    }
    ListNode* p = head;
    for(int i = 0 ; i < index - 1 ; i++) {
        p = p->next;
    }
    ListNode* temp = p->next;
    p->next = temp->next;
    delete temp;
    size--;
}
```

## 總結

這一題基本上把最常用的六個Linked list的功能給實作出來了，之後的有些題目也是會從這些function中延伸出來。

其中deleteAtIndex中，很多人在實作的時候會忘記delete掉被刪除的節點，雖然只佔了一點點的空間，但是還是要刪除，避免之後有很多節點的時候，會對記憶體空間造成負擔。

當然如果使用像是python這種會自動分配、更新記憶體的程式語言是，就不用手動刪除，但既然這是C++，還是要delete掉喔！
