# LeetCode: 102. 二叉树的层次遍历

[TOC]



## 1、题目描述



​	给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。

例如:
给定二叉树: `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```



## 2、解题思路

​	借助数据结构，如队列，存储当前行的节点

​	然后将当前行的节点的子节点放入队列中，并将当前节点弹出，放入结果数组中

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

import queue

class Solution:
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []

        result = []
        q = queue.Queue()

        q.put(root)
        while not q.empty():
            temp = []
            size = q.qsize()
            for i in range(size):
                node = q.get()
                if node.left:
                    q.put(node.left)
                if node.right:
                    q.put(node.right)

                temp.append(node.val)
            result.append(temp)
        return result
```







