# LeetCode: 142. 环形链表 II

[TOC]



## 1、题目描述

​	给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

**说明：**不允许修改给定的链表。

**进阶：**
	你是否可以不用额外空间解决此题？



## 2、解题思路

​	在前面循环链表中，使用快慢指针的技巧，判断是否存在环，这道题可以分为两步：

- 是否存在环

  使用快慢指针来做

- 找出环的第一个节点

  如果存在环，那么快指针比慢指针肯定是多走了一圈，那么记住相遇的节点，头结点与这个节点同时开始走，相遇的地方，就是环的第一个节点

  

  ```python
  # Definition for singly-linked list.
  # class ListNode(object):
  #     def __init__(self, x):
  #         self.val = x
  #         self.next = None
  
  class Solution(object):
      def detectCycle(self, head):
          """
          :type head: ListNode
          :rtype: ListNode
          """
          
          if not head:
              return head
  
          faster = head
          slower = head
          meeting = None
  
          while faster:
              faster = faster.next
              if faster:
                  faster = faster.next
                  slower = slower.next
  
              else:
                  return None
  
              if faster == slower:
                  meeting = faster
                  break
  
          if meeting is not None:
              while meeting != head:
                  meeting = meeting.next
                  head = head.next
  
          return meeting
  ```

  

  