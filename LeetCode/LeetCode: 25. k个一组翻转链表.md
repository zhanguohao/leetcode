# LeetCode: 25. k个一组翻转链表

[TOC]



## 1、题目描述

​	

给出一个链表，每 *k* 个节点一组进行翻转，并返回翻转后的链表。

*k* 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 *k* 的整数倍，那么将最后剩余节点保持原有顺序。

**示例 :**

给定这个链表：`1->2->3->4->5`

当 *k* = 2 时，应当返回: `2->1->4->3->5`

当 *k* = 3 时，应当返回: `3->2->1->4->5`

**说明 :**

- 你的算法只能使用常数的额外空间。
- **你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

## 2、解题思路

​	实际上这道题是比较简单的

- 首先，设置一个头指针

- 然后，截取一段长度为k的链表进行翻转，然后接起来

- 如果长度小于k，直接接起来即可

  

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseKGroup(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        if k == 1:
            return head

        res = ListNode(0)
        cur = res

        begin = head
        end = begin
        count = 1
        while cur:
            count = 1
            end = begin
            while count < k and end:
                end = end.next
                count += 1
            if count >= k and end:
                next_section = begin
                # 截取一段，最后一个next设置为None
                first = begin
                begin = end.next
                end.next = None

                middle = first.next

                while middle:
                    third = middle.next
                    middle.next = first
                    first = middle
                    middle = third

                cur.next = first
                cur = next_section
            else:
                cur.next = begin
                cur = None
        return res.next
```

​	主要是几个细节需要注意，截取的时候，如果已经到了None，表示长度已经不够，这时候，也需要直接接起来

