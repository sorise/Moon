### [LinkList 链表](#)

**介绍**：无需多言，用得多常见的数据结构了，不了解它的特性，当什么程序员。

----







#### [1. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

必背操作，将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

[leetcode 链接： https://leetcode.cn/problems/merge-two-sorted-lists/](https://leetcode.cn/problems/merge-two-sorted-lists/)

```cpp
//递归
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    if(list1 == nullptr) return list2;
    if(list2 == nullptr) return list1;

    if(list1->val <= list2->val){
        list1->next = mergeTwoLists(list1->next, list2);
        return list1;
    }else{
        list2->next = mergeTwoLists(list1, list2->next);
        return list2;
    }
}

//非递归，临时头结点
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    if(list1 == nullptr) return list2;
    if(list2 == nullptr) return list1;

    ListNode * head = new ListNode(-1); //临时头节点
    ListNode * prev = head;
    //找下一个节点
    while(list1 && list2){
        if(list1->val <= list2->val){
            prev->next = list1;
            list1 = list1->next;
        }else{
            prev->next = list2;
            list2 = list2->next;
        }
        prev = prev->next;
    }

    if(list2 == nullptr) prev->next = list1;
    if(list1 == nullptr) prev->next = list2;

    auto rv = head->next;
    delete head;
    return rv;
}
```



#### 2. 归并排序链表

[leetcode 链接： https://leetcode.cn/problems/sort-list/description/](https://leetcode.cn/problems/sort-list/description/)

空间：(nlogn)

```cpp
//必须归并排序
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode* ans = mergeSort(head, nullptr);
        return ans;
    }

    ListNode* mergeSort(ListNode* head, ListNode* tail){
        if(head == tail){
            return head;
        }
        if(head->next == tail){
            head->next = nullptr;
            return head;
        }

        // findMid
        ListNode *slow = head, *fast = head;
        while(fast != tail && fast->next != tail){
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode *mid = slow;
        ListNode *pre = mergeSort(head, mid);
        ListNode *after = mergeSort(mid, tail);

        // merge
        ListNode *hair = new ListNode(-1), *p = hair;
        while(pre && after){
            if(pre->val < after->val){
                p->next = pre;
                pre = pre->next;
            }else{
                p->next = after;
                after = after->next;
            }
            p = p->next;
        }
        if(pre) p->next = pre;
        if(after) p->next = after;
        return hair->next;
    }
};
```

#### C++ 归并排序

由于题目要求空间复杂度是 O(1)，因此不能使用递归。因此这里使用 bottom-to-up 的算法来解决。

太晚了，明天讲解！

ok，归来！

bottom-to-up 的归并思路是这样的：先两个两个的 merge，完成一趟后，再 4 个4个的 merge，直到结束。举个简单的例子：`[4,3,1,7,8,9,2,11,5,6]`.

```rust
step=1: (3->4)->(1->7)->(8->9)->(2->11)->(5->6)
step=2: (1->3->4->7)->(2->8->9->11)->(5->6)
step=4: (1->2->3->4->7->8->9->11)->(5->6)
step=8: (1->2->3->4->5->6->7->8->9->11)
```

链表里操作最难掌握的应该就是各种断链啊，然后再挂接啊。在这里，我们主要用到链表操作的两个技术：

- `merge(l1, l2)`，双路归并，我相信这个操作大家已经非常熟练的，就不做介绍了。
- `cut(l, n)`，可能有些同学没有听说过，它其实就是一种 split 操作，即断链操作。不过我感觉使用 cut 更准确一些，它表示，将链表 `l` 切掉前 n 个节点，并返回后半部分的链表头。
- 额外再补充一个 dummyHead 大法，已经讲过无数次了，仔细体会吧。

------

希望同学们能把双路归并和 cut 断链的代码烂记于心，以后看到类似的题目能够刷到手软。

掌握了这三大神器后，我们的 bottom-to-up 算法伪代码就十分清晰了：

```javascript
current = dummy.next;
tail = dummy;
for (step = 1; step < length; step *= 2) {
	while (current) {
		// left->@->@->@->@->@->@->null
		left = current;

		// left->@->@->null   right->@->@->@->@->null
		right = cut(current, step); // 将 current 切掉前 step 个头切下来。

		// left->@->@->null   right->@->@->null   current->@->@->null
		current = cut(right, step); // 将 right 切掉前 step 个头切下来。
		
		// dummy.next -> @->@->@->@->null，最后一个节点是 tail，始终记录
		//                        ^
		//                        tail
		tail.next = merge(left, right);
		while (tail->next) tail = tail->next; // 保持 tail 为尾部
	}
}
```

好了，下面是比较正式的代码。

```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode dummyHead(0);
        dummyHead.next = head;
        auto p = head;
        int length = 0;
        while (p) {
            ++length;
            p = p->next;
        }
        
        for (int size = 1; size < length; size <<= 1) {
            auto cur = dummyHead.next;
            auto tail = &dummyHead;
            
            while (cur) {
                auto left = cur;
                auto right = cut(left, size); // left->@->@ right->@->@->@...
                cur = cut(right, size); // left->@->@ right->@->@  cur->@->...
                
                tail->next = merge(left, right);
                while (tail->next) {
                    tail = tail->next;
                }
            }
        }
        return dummyHead.next;
    }
    
    ListNode* cut(ListNode* head, int n) {
        auto p = head;
        while (--n && p) {
            p = p->next;
        }
        
        if (!p) return nullptr;
        
        auto next = p->next;
        p->next = nullptr;
        return next;
    }
    
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode dummyHead(0);
        auto p = &dummyHead;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                p->next = l1;
                p = l1;
                l1 = l1->next;       
            } else {
                p->next = l2;
                p = l2;
                l2 = l2->next;
            }
        }
        p->next = l1 ? l1 : l2;
        return dummyHead.next;
    }
};
```
