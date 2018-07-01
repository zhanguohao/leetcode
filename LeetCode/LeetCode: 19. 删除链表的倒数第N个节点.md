# LeetCode: 19. 删除链表的倒数第N个节点

[TOC]

## 1、题目描述



给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**

给定的 *n* 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？





## 2、解题思路

​	

​	这个题目有一个小技巧在里面，就是如何使用一趟扫描找到想要找的元素呢，就是使用双指针

​	先让第二个指针走n步，然后两个指针一起走，这样第一个指针指向的就是待删除的元素



​	下面的处理方式是让走N+1步，这样就知道待删除的前面的一个元素，

​	不过还需要判断一下如果待删除的长度与整个链表长度一样，表示要删除的是头结点

​	如果不一样，就是要删除头结点下一个节点

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        count = -1
        length = 0
        temp = head
        target = head
        while temp:
            if count >= n:
                target = target.next
            else:
                count += 1
            temp = temp.next
            length += 1

        if length == n:
            return head.next
        else:
            target.next = target.next.next
        return head
```

