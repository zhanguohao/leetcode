# LeetCode: 206. 反转链表

[TOC]

## 1、题目描述





反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**进阶:**
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？



## 2、解题思路

### 2.1 迭代法

​	需要注意的是每一次保存下一个指针



```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverseList(struct ListNode* head) {
    if (!head || !head->next) {
        return head;
    }

    struct ListNode *node_next = head->next;
    struct ListNode *node_prev = NULL;

    while (node_next) {
        head->next = node_prev;
        node_prev = head;
        head = node_next;
        node_next = node_next->next;
    }
    head->next = node_prev;
    return head;
}
```

