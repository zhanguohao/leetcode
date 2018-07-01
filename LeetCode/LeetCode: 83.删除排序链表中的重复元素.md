# LeetCode: 83.删除排序链表中的重复元素

[TOC]



## 1、题目描述

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

**示例 1:**

```
输入: 1->1->2
输出: 1->2
```

**示例 2:**

```
输入: 1->1->2->3->3
输出: 1->2->3
```



## 2、解题思路

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* deleteDuplicates(struct ListNode* head) {
    // 如果只有一个元素或为空
    if (!head || !head->next ) {
        return head;
    }
    struct ListNode * result = head;
    while (head->next) {
        if (head->val == head->next->val) {
            head->next = head->next->next;
        } else {
            head = head->next;
        }
    }
    
    return result;
}
```



```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* deleteDuplicates(struct ListNode* head) {
     // 如果只有一个元素或为空
    if (!head || !head->next) {
        return head;
    }
    struct ListNode *cur = head;
    struct ListNode *ne = head;

    if (head) {
        ne = head->next;
    }
    while (ne) {

        if(cur->val == ne->val){
            cur->next = ne->next;
            ne = ne->next;
        } else{
            cur = cur->next;
            ne = ne->next;
        }
    }

    return head;
}
```

