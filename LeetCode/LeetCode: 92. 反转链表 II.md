# LeetCode: 92. 反转链表 II

[TOC]



## 1、题目描述





反转从位置 *m* 到 *n* 的链表。请使用一趟扫描完成反转。

**说明:**
1 ≤ *m* ≤ *n* ≤ 链表长度。

**示例:**

```
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```



## 2、解题思路

​	设置两个指针，第一个指针先走m步然后两个指针一块走一面走，一面反转

​	考虑到头部的元素也可能反转，所以要在最前面添加一个节点，用来指向返回的头结点

​	

​	

```python
class Solution:
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """
        
        prev = ListNode(0)
        result_head = prev
        prev.next = head

        temp = m

        while temp > 1:
            prev = prev.next
            temp -= 1

        first_reverse = prev.next

        first = prev.next
        second = first.next
        temp_node = second
        if temp_node:
            temp_node = first.next.next

        temp = n
        while temp - m > 0:
            second.next = first
            first = second
            second = temp_node
            if temp_node:
                temp_node = temp_node.next
            temp -= 1

        prev.next = first
        first_reverse.next = second
        return result_head.next
        
```





​	