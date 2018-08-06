# LeetCode: 23. 合并K个排序链表

[TOC]

## 1、题目描述

合并 *k* 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**示例:**

```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```



## 2、解题思路

​	实际上，这个题目很简单，仅仅需要注意的就是，我们每一次取出来的那个都是值最小的

​	因此，我们使用堆排序，每一次取出最小的那个头指针，降低时间复杂度

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        heap = [(x.val, x) for x in lists if x]
        heapq.heapify(heap)

        res = ListNode(0)
        cur = res
        while heap:
            node = heapq.heappop(heap)
            cur.next = node[1]
            cur = cur.next
            next_node = node[1].next
            if next_node:
                heapq.heappush(heap, (next_node.val, next_node))
        cur.next = None
        return res.next
```

