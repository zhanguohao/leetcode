# LeetCode: 148. 排序链表

[TOC]

## 1、题目描述



在 *O*(*n* log *n*) 时间复杂度和常数级空间复杂度下，对链表进行排序。

**示例 1:**

```
输入: 4->2->1->3
输出: 1->2->3->4
```

**示例 2:**

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```



## 2、解题思路

​	前面刚刚过做了插入排序，可惜插入排序的时间复杂度是$O(n^2)$的，并不是题目要求的

​	题目要求的时间复杂度，一般能想到三种排序，快排，堆排序，归并排序，堆排序很复杂，也需要额外空间，快排引入了递归，所以归并排序是比较合适的

​	首先我们要获取链表的长度，然后归并的步伐从1开始，然后是2，4，8，16

​	不过这样实现有点麻烦，换个思路

- 首先给定一个链表，分成两部分，假定这两部分已经排序好，调用归并排序
- 左面这一部分，递归调用归并排序
- 右面这部分，递归调用归并排序
- 返回归并排序结果



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def sortList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        
        if not head or not head.next:
            return head

        first = head

        faster = head
        slower = head
        # 将链表分成两部分
        while faster:
            faster = faster.next
            if faster:
                faster = faster.next
                if faster:
                    slower = slower.next

        second = slower.next
        slower.next = None

        part1 = self.sortList(first)
        part2 = self.sortList(second)

        return self.merge(part1, part2)
	# 链表归并
    def merge(self, first, second):

        result = ListNode(0)
        result.next = first

        prev = result
        while first or second:

            if first and second:
                if first.val < second.val:
                    prev.next = first
                    first = first.next
                else:
                    prev.next = second
                    second = second.next
                prev = prev.next
            if not first:
                prev.next = second
                break
            if not second:
                prev.next = first
                break
        return result.next
```



