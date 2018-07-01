# LeetCode: 61. 旋转链表

[TOC]

## 1、题目描述

​	



给定一个链表，旋转链表，将链表每个节点向右移动 *k* 个位置，其中 *k* 是非负数。

**示例 1:**

```
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
```

**示例 2:**

```
输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL
```



## 2、解题思路

​	这个链表旋转实在是很简单的题目，如果将链表头尾相连，直接然后找到尾部，加上NULL，返回头部指针就可以了

​	需要注意的就是，如果旋转长度超过了链表长度，我们需要取余处理



在前面，19题，删除链表的倒数第N个节点中，我们使用了双指针来找到第N个需要删除的节点，这里也是这样做

​	第一个指针先走n步，然后第二个指针走到头，这样我们就得到了倒数第n个元素，将这个元素设为头指针，然后将尾部与头部相连，就可以了

​	需要注意的就是边界条件

​	

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        if not head:
            return head
        
        
        first = head
        second = head
        total = 0
        temp = head
        temp_k = k
        result = None
        while temp_k > 0 and temp:
            temp = temp.next
            temp_k -= 1

        if temp_k == 0 and temp is None:
            return head

        if temp_k == 0 and temp is not None:
            first = temp

        if temp_k > 0 and temp is None:
            total = k - temp_k
            temp_k = k % total

            while temp_k > 0:
                first = first.next
                temp_k -= 1

        while first.next:
            first = first.next
            second = second.next
        first.next = head
        result = second.next
        second.next = None

        return result
```

