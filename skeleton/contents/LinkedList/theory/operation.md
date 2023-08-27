

```cpp
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```
#### [删除重复节点](#)
后删除或者前删除

后删除
```cpp
ListNode* deleteDuplicates(ListNode* head) {
    if (head == nullptr|| head->next == nullptr) return head;
    ListNode* prev = head;
    ListNode* now = head->next;
    while (now != nullptr){
        if (now->val == prev->val){
            //删除当前节点
            prev->next = now->next;
            now = now->next;

        }else{
            prev = now;
            now = now->next;
        }
    }
    return head;
}
```
前删除
```cpp
ListNode* deleteDuplicates(ListNode* head) {
    if (head == nullptr || head->next == nullptr) return head;
    ListNode* cur = head;
    while (cur->next != nullptr){
        if (cur->val == cur->next->val){
            cur->next = cur->next->next;
        }else{
            cur = cur->next;
        }
    }
    return head;
}
```

#### [删除第N个节点](#)
这看起来是一个很简单的问题，一般思路是找到这个节点的前驱节点然后进行删除操作就可以了，但是也暗藏玄机。
* N是从0开始的，还是从1开始的，由于程序是给程序员设计的，所以应该从O开始！
* 问题在于删除第0个节点和删除第1节点的区别，首节点没有前驱，而第一个节点的前驱就是首节点，如何解决这个问题是关键！

```cpp
//删除第N个节点
ListNode* removeNthNode(ListNode* head, int n) {
    //首先找到第 n-1 这个节点
    ListNode* prev = nullptr; //前驱节点
    for (int i = 0; i < n ; ++i) {
        if (prev == nullptr) prev = head;
        else prev = prev->next;
    }
    if (prev == nullptr) {
        head = head->next;
    }else{
        prev->next = prev->next->next;
    }
    return head;
}
```

#### [删除倒数第N个节点](#)
利用上面的函数，但是这个N得是从1开始！ 还可以利用栈来解决！

便利迭代解法！
```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode *temp = head;
    int count = 0;
    while (temp != nullptr){
        count++;
        temp = temp->next;
    }
    return removeNthNode(head, count - n);
}
```

栈的方法, 但是要注意对栈空的判断，注意输入是 `head = [1], n = 1` 这种情况的判断！
```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode *temp = head;
    std::stack<ListNode *> values;
    while (head != nullptr){
        values.push(head);
        head = head->next;
    }

    for (int i = 0; i < n; ++i) {
        if (!values.empty()) values.pop();
    }
    if (!values.empty()){
        values.top()->next = values.top()->next->next;
    }else{
        temp = temp->next;
    }
    return temp;
}
```