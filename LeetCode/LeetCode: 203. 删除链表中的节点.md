# LeetCode: 203. 删除链表中的节点

[TOC]

## 1、题目描述

删除链表中等于给定值 **val** 的所有节点。

**示例:**

```
输入: 1->2->6->3->4->5->6, val = 6
输出: 1->2->3->4->5
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
struct ListNode* removeElements(struct ListNode* head, int val) {
    
     if (!head) {
        return head;
    }

    while (head && head->val == val) {
        head = head->next;
    }

    struct ListNode *prev_node;
    struct ListNode *cur_node;
    if (head) {
        prev_node = head;
        cur_node = head->next;
    } else {
        return head;
    }

    while (cur_node) {
        if (cur_node->val == val) {
            prev_node->next = cur_node->next;
            cur_node = cur_node->next;
        } else {
            cur_node = cur_node->next;
            prev_node = prev_node->next;
        }
    }

    return head;
    
    
    
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
struct ListNode* removeElements(struct ListNode* head, int val) {
    
    
     if (!head) {
        return head;
    }
    struct ListNode *prev_node = head;
    struct ListNode *cur_node = head->next;

    while (cur_node) {
        if (cur_node->val == val) {
            prev_node->next = cur_node->next;
            cur_node = cur_node->next;
        } else {
            cur_node = cur_node->next;
            prev_node = prev_node->next;
        }
    }
    
    if(head->val==val){
        head = head->next;
    }

    return head;
    
    
}
```

