# LeetCode: 141. 环形链表

[TOC]

## 1、 题目描述



给定一个链表，判断链表中是否有环。

**进阶：**
你能否不使用额外空间解决此题？





## 2、解题思路

​	从开头向后扫描

​	如果遇到NULL，表示没有环形的

​	然后设置两个指针

​	一个指针一次前进一步，另一个一次前进两步

​	如果有环形，那么这两个指针肯定能够相遇

​	，就好比一个车跑得快，一个跑得慢，在环形里面，跑得快的肯定能追上跑得慢的



```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
bool hasCycle(struct ListNode *head) {
    
    
    
    if (head == NULL || head->next == NULL) {
        return false;
    }
    struct ListNode *current = head;
    struct ListNode *next_node = head->next;

    while (current != next_node) {
        if (next_node == NULL || next_node->next == NULL) {
            return false;
        }
        current = current->next;
        next_node = next_node->next->next;
    }
    return true;
    
}
```

​	需要注意的是，判断跳出循环的条件用跑的快的节点判断，放置出现下一个节点已经是结尾的情况，next_node就会赋值出错的

```c
        if (next_node == NULL || next_node->next == NULL) {
            return false;
        }
```

