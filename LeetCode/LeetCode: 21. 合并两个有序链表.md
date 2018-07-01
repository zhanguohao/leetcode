# LeetCode: 21. 合并两个有序链表

[TOC]

## 1、题目描述

```
将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```



## 2、解题思路

## 2.1 向前判断

​	设置头指针和临时指针

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* mergeTwoLists(struct ListNode* l1, struct ListNode* l2) {
    // 返回用的头指针，指向数据小的元素的头元素
    struct ListNode *head;
    if (l1 == NULL || l2 == NULL) {
        if (l1 == NULL) {

            head = l2;
        } else {
            head = l1;
        }
        return head;
    }

    head = l1->val > l2->val ? l2 : l1;
    struct ListNode *temp = head;
    if (l1->val > l2->val) {
        l2 = l2->next;
    } else {
        l1 = l1->next;
    }


    while (l1 != NULL || l2 != NULL) {
        if (l1 != NULL && l2 == NULL) {
            temp->next = l1;
            break;
        } else if (l1 == NULL && l2 != NULL) {
            temp->next = l2;
            break;
        } else {
            if (l1->val > l2->val) {
                temp = temp->next = l2;
                l2 = l2->next;

            } else {
                temp = temp->next = l1;
                l1 = l1->next;
            }
        }
    }

    return head;    


}
```

## 2.2 递归法

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode *mergeTwoLists(struct ListNode *l1, struct ListNode *l2) {
    if (l1 == NULL){
        return l2;
    }
    if (l2 == NULL){
        return l1;
    }
    if (l1->val < l2->val){
        l1->next = mergeTwoLists(l1->next,l2);
        return l1;
    }else{
        l2->next = mergeTwoLists(l2->next,l1);
        return l2;
    }
}

```

