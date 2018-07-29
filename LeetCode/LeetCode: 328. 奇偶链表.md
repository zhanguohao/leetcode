# LeetCode: 328. 奇偶链表

[TOC]



## 1、题目描述

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

**示例 1:**

```
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
```

**示例 2:**

```
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
```

**说明:**

- 应当保持奇数节点和偶数节点的相对顺序。
- 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。

## 2、解题思路

​	这个是比较简单的，直接设置两个指针，然后第一个指向奇数的，第二个指向偶数的，扫描一遍，连接起来即可

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def oddEvenList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        odd = ListNode(1)
        odd_pos = odd
        even = ListNode(2)
        even_pos = even

        temp = head
        count = 1
        while temp:
            if count % 2 == 0:
                even_pos.next = temp
                even_pos = even_pos.next
            else:
                odd_pos.next = temp
                odd_pos = odd_pos.next
            temp = temp.next
            count += 1

        even_pos.next = None
        odd_pos.next = even.next

        return odd.next
```

