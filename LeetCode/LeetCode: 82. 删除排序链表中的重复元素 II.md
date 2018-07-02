# LeetCode: 82. 删除排序链表中的重复元素 II

[TOC]



## 1、题目描述





给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 *没有重复出现* 的数字。

**示例 1:**

```
输入: 1->2->3->3->4->4->5
输出: 1->2->5
```

**示例 2:**

```
输入: 1->1->1->2->3
输出: 2->3
```



## 2、解题思路

​	设置两个指针，如果第一个指针指向的节点的值等于第二个指针指向的节点的值，就将将第二个指针后移，一直移到不相等或者为空，

​	然后，这一段就是需要删除的

​	这个题目的关键点就在与，头指针的判定，我们需要构造一个节点，这个节点指向头指针

​	然后从头指针开始删除

​	一开始，prev指向的不是头指针，而是我们构造的节点，因为头部的元素也可能是需要删除的	



​        **注意，prev指向前面一个有效节点**

---





​	使用p，q两个指针进行判断，如果两个值相等，表示遇到了需要删除的，就让q继续向下走，直到找到不相等的那个或者None为止，这时候，中间的一段需要删除；

​	然后令prev的next指向q，这样就删掉了中间的重复节点，然后继续判断

​	如果下一个值与当前的值不相等，表示当前的值是一个有效节点，prev指向这个节点

​	以此类推，不断地向下判断



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteDuplicates(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        
        if not head or not head.next:
            return head

        prev = ListNode(0)
        prev.next = head

        result_head = prev

        p = head
        q = head.next

        while p and q:

            if p.val == q.val:
                while q.next:
                    if p.val != q.val:
                        break
                    q = q.next
                if p.val == q.val:
                    prev.next = None
                    break
                else:
                    prev.next = q
                    p = q
                    q = q.next
            else:
                prev = p
                p = q
                q = q.next

        return result_head.next
        
```



​	