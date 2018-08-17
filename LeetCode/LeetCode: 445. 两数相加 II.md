# LeetCode: 445. 两数相加 II

[TOC]

## 1、题目描述

给定两个**非空**链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储单个数字。将这两数相加会返回一个新的链表。

 

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

**进阶:**

如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

**示例:**

```
输入: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
输出: 7 -> 8 -> 0 -> 7
```

## 2、解题思路

​	一般的会将这两个数字拿出来，然后计算，再然后就是转换成链表返回

​	直接使用双向链表，还是比较简单的

​	看到别人直接转换为整数然后计算，不过这样有些取巧，不建议，很容易溢出；

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
import queue

class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        q1 = queue.deque()
        q2 = queue.deque()

        def to_queue(l, q):
            while l:
                q.appendleft(l.val)
                l = l.next

        def plus(ql1, ql2):
            qp = queue.deque()
            carry = 0
            while ql1 or ql2:
                if ql1 and ql2:
                    digit_sum = ql1.popleft() + ql2.popleft() + carry
                elif ql1:
                    digit_sum = ql1.popleft() + carry
                else:
                    digit_sum = ql2.popleft() + carry
                digit = digit_sum % 10
                carry = digit_sum // 10
                qp.append(digit)
            if carry:
                qp.append(carry)
            return qp

        def to_list(q):
            head = ListNode(0)
            cur = head
            while q:
                cur.next = ListNode(q.pop())
                cur = cur.next
            cur.next = None
            return head.next

        to_queue(l1, q1)
        to_queue(l2, q2)

        return to_list(plus(q1, q2))
```

​	