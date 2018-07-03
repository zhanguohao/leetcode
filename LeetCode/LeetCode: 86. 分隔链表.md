# LeetCode: 86. 分隔链表

[TOC]

## 1、题目描述

​	给定一个链表和一个特定值 *x*，对链表进行分隔，使得所有小于 *x* 的节点都在大于或等于 *x* 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

**示例:**

```
输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5
```



## 2、解题思路

​	分成两个链表，第一个链表保存小于x的所有节点

​	第二个链表保存大于等于x的链表

​	然后将两个链表合并即可



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def partition(self, head, x):
        """
        :type head: ListNode
        :type x: int
        :rtype: ListNode
        """
        if not head or not head.next:
            return head

        less = ListNode(0)
        less_p = less

        big_equal_than = ListNode(0)
        big_equal_p = big_equal_than

        temp = head

        while temp:
            if temp.val < x:
                less_p.next = temp
                less_p = less_p.next
            else:
                big_equal_p.next = temp
                big_equal_p = big_equal_p.next
            temp = temp.next

        less_p.next = big_equal_than.next
        big_equal_p.next = None

        return less.next
```

