# LeetCode: 430. Flatten a Multilevel Doubly Linked List

[TOC]

## 1、题目描述

You are given a doubly linked list which in addition to the next and previous pointers, it could have a child pointer, which may or may not point to a separate doubly linked list. These child lists may have one or more children of their own, and so on, to produce a multilevel data structure, as shown in the example below.

Flatten the list so that all the nodes appear in a single-level, doubly linked list. You are given the head of the first level of the list.

**Example:**

```
Input:
 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL

Output:
1-2-3-7-8-11-12-9-10-4-5-6-NULL
```

**Explanation for the above example:**

Given the following multilevel doubly linked list:

![img](https://leetcode-cn.com/static/images/problemset/MultilevelLinkedList.png)

We should return the following flattened doubly linked list:

![img](https://leetcode-cn.com/static/images/problemset/MultilevelLinkedListFlattened.png)

## 2、解题思路

​	可以使用深度优先搜索，也可以不是用，首先来剖析一下问题

​	这道题与前面114题很相似，如果当前的节点有子节点，该如何做呢？

​	将剩下的节点，挂到子节点的末尾，然后，将当前节点的next指向子节点，子节点的prev指向当前节点即可，然后节点后移，继续判断

```python
class Solution(object):
    def flatten(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        ans = head

        while head:
            if head.child:
                head_next = head.next
                temp = head.child
                while temp.next:
                    temp = temp.next
                temp.next = head_next
                head_next.prev = temp

                head.next = head.child
                head.child.prev = head
                head.child = None

            head = head.next

        return ans

```

​	思路很简单，但是通过了19个测试用了，下一个超时了，因此，要考虑如何提高效率

​	考虑在展开的过程中，不直接将下一个元素挂到子列的尾部，而是保存到栈中，等到遍历到最后的时候，就将栈中的元素取出来，接到尾部，然后继续判断即可

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val, prev, next, child):
        self.val = val
        self.prev = prev
        self.next = next
        self.child = child
"""
class Solution(object):
    def flatten(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        ans = head
        if not head:
            return ans

        stack = []
        prev = None
        while head or stack:
            if head:
                prev = head
                if head.child:
                    if head.next:
                        stack.append(head.next)
                    head.next = head.child
                    head.child.prev = head
                    head.child = None
                head = head.next
            else:
                temp = stack.pop()
                prev.next = temp
                temp.prev = prev
                head = temp

        return ans
```

​	这样，仅需要从头到尾遍历一遍即可得出结果