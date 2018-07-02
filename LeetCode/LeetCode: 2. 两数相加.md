# LeetCode: 2. 两数相加

[TOC]

## 1、题目描述

```
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

​	给定两个**非空**链表来表示两个非负整数。位数按照**逆序**方式存储，它们的每个节点只存储单个数字。将两数相加返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

**示例：**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

## 2、解题思路

​	这个问题很简单，我们设计两个指针，指向两个链表，并且保存一个进位就可以了，在不断的前进过程中，不断地更新当前位置与进位

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2) {
    
    struct ListNode *result=NULL,*temp,*p1,*p2;
    int carry=0,sum,x,y;
    for(p1=l1,p2=l2;p1 || p2;){
        x = p1!=NULL?p1->val:0;
        y = p2!=NULL?p2->val:0;
        
        if(result == NULL){
            result = temp = (struct listNode*)malloc(sizeof(struct ListNode));
        } else{
            temp =  temp->next = (struct listNode*)malloc(sizeof(struct ListNode));
        }
        
        temp->val = (x+y+carry)%10;
        temp->next = NULL;
        
        carry = (x+y+carry)>9?1:0;
        if(p1){
            p1 = p1->next;
        }
        if(p2){
            p2 = p2->next;
        }
    }
    if (carry){
        temp =  temp->next = (struct listNode*)malloc(sizeof(struct ListNode));
        temp->val = 1;
        temp->next = NULL;
    }
        
    return result;
        
}
```

