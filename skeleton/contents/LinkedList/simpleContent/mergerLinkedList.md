### [simple. 合并两个有序链表](#)
**题目**: 将两个升序链表合并为一个新的 升序 链表并返回。新:链表是通过拼接给定的两个链表的所有节点组成的。 [LeetCode ->](https://leetcode.cn/problems/merge-two-sorted-lists/)

----

```cpp
struct ListNode {
     int val;
     ListNode *next;
     ListNode() : val(0), next(nullptr) {}
     ListNode(int x) : val(x), next(nullptr) {}
     ListNode(int x, ListNode *next) : val(x), next(next) {}
};
```

**测试用例**
```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

```
输入：l1 = [], l2 = []
输出：[]
```

```
输入：l1 = [], l2 = [0]
输出：[0]
```

#### [解题思路](#)
首先是迭代，其次是递归!

迭代解法:
```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    ListNode* head = new ListNode(-1, nullptr);
    ListNode* merge = head;
    while (list1 != nullptr && list2 != nullptr){
        if (list1->val < list2->val){
            merge->next = list1;
            list1 = list1->next;
        }else {
            merge->next = list2;
            list2 = list2->next;
        }
        merge = merge->next;
    }
    merge->next = (list1 == nullptr)?list2: list1;
    ListNode* top = head->next;
    delete head;
    return top;
}
```


递归解法：
```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    if (list1 == nullptr) return list2;
    else if (list2 == nullptr) return list1;
    else if (list1->val < list2->val) {
    list1->next = mergeTwoLists(list1->next, list2);
    return list1;
    }else{
        list2->next = mergeTwoLists(list1, list2->next);
        return list2;
    }
}
```

#### [自己的垃圾解法](#)
```cpp
class Solution {
public:
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    ListNode *top = nullptr;
    if (list1 == nullptr && list2 != nullptr) {
        top = list2;
        list2 = list2->next;
    }
    else if (list2 == nullptr && list1 != nullptr){
        top = list1;
        list1 = list1->next;
    }
    else if(list1 == nullptr && list2 == nullptr){
        return top;
    }
    else {
        if (list1->val < list2->val){
            top = list1;
            list1 = list1->next;
        }else {
            top = list2;
            list2 = list2->next;
        }
    };

    ListNode * merger = top;
    while (list1 != nullptr || list2 != nullptr){
        if (list1 != nullptr && list2 != nullptr){
            if (list1->val < list2->val){
                merger->next = list1;
                merger = list1;
                list1 = list1->next;
            }else {
                merger->next = list2;
                merger = list2;
                list2 = list2->next;
            }
            continue;
        }

        if(list2 == nullptr && list1 != nullptr ){
            merger->next = list1;
            merger = list1;
            list1 = list1->next;
            continue;
        }

        if (list1 == nullptr && list2 != nullptr ){
            merger->next = list2;
            merger = list2;
            list2 = list2->next;
            continue;
        }
    }
    return top;
}
};
```