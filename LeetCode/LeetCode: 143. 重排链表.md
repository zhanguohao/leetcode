# LeetCode: 143. 重排链表

[TOC]



## 1、题目描述



给定一个单链表 *L*：*L*0→*L*1→…→*L**n*-1→*L*n ，
将其重新排列后变为： *L*0→*L**n*→*L*1→*L**n*-1→*L*2→*L**n*-2→…

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**示例 1:**

```
给定链表 1->2->3->4, 重新排列为 1->4->2->3.
```

**示例 2:**

```
给定链表 1->2->3->4->5, 重新排列为 1->5->2->4->3.
```



## 2、解题思路

​	这道题目首先确定需要找到中间节点，然后从前面开始，每一个节点都需要插入一个节点

​	一种做法是先找到中间节点，然后将后面的一半反转，从前面开始向后插入

​	还可以直接放到堆栈中，每一次从堆栈里面取一个节点插入

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None


class Solution(object):
    def reorderList(self, head):
        """
        :type head: ListNode
        :rtype: void Do not return anything, modify head in-place instead.
        """
        if not head:
            return head

        d = []
        temp = head
        while temp:
            d.append(temp)
            temp = temp.next

        length = len(d)
        current = head
        next_node = current.next
        for i in range((length - 1) // 2):
            node = d.pop()
            current.next = node
            node.next = next_node
            current = next_node
            next_node = current.next

        if length % 2 == 0:
            next_node.next = None
        else:
            current.next = None
```

​	在python3中，有queue，不过在python2中，直接使用list模拟即可

