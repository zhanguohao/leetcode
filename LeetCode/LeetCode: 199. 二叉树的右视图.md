# LeetCode: 199. 二叉树的右视图

[TOC]



## 1、题目描述



给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

**示例:**

```
输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```



## 2、解题思路

​	这道题很好解，就是所有层次遍历的时候，将最后一个元素放到里面即可

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
import queue

class Solution:
    def rightSideView(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        result = []
        d = queue.deque()
        d.append(root)
        while d:
            result.append(d[-1].val)
            size = len(d)

            for i in range(size):
                node = d.popleft()
                if node.left:
                    d.append(node.left)
                if node.right:
                    d.append(node.right)
        return result
```