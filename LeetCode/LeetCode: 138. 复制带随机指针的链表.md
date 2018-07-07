# LeetCode: 138. 复制带随机指针的链表

[TOC]



## 1、题目描述

​	

给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

要求返回这个链表的深度拷贝。 



## 2、解题思路

​	这道题的深度拷贝，主要需要注意的问题就是不能形成环，也就是已经生成的节点不需要再次创建

```python
# Definition for singly-linked list with a random pointer.
# class RandomListNode(object):
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None

class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: RandomListNode
        :rtype: RandomListNode
        """
        result = {}
        return self.dfs(head, result)

    def dfs(self, node, result):
        if node:
            if result.get(node) is None:
                result[node] = RandomListNode(node.label)
                result[node].next = self.dfs(node.next,result)
                result[node].random = self.dfs(node.random,result)
        else:
            return None

        return result[node]
```

