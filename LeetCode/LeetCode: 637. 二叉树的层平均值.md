# LeetCode: 637. 二叉树的层平均值

[TOC]

## 1、题目描述



给定一个非空二叉树, 返回一个由每层节点平均值组成的数组.

**示例 1:**

```
输入:
    3
   / \
  9  20
    /  \
   15   7
输出: [3, 14.5, 11]
解释:
第0层的平均值是 3,  第1层是 14.5, 第2层是 11. 因此返回 [3, 14.5, 11].
```

**注意：**

1. 节点值的范围在32位有符号整数范围内。



## 2、解题思路



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from queue import Queue
class Solution:
    def averageOfLevels(self, root):
        """
        :type root: TreeNode
        :rtype: List[float]
        """
        
        if root is None:
            return [];

        q = Queue()
        q.put(root)
        result = []
        while not q.empty():
            nums = q.qsize()
            sums = 0
            for i in range(nums):
                node = q.get()
                sums += node.val
                if node.left is not None:
                    q.put(node.left)
                if node.right is not None:
                    q.put(node.right)
            result.append(sums / nums)

        return result
        
       
```





​	有看了一个别人的思路，果然python理解的还不够好

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from queue import Queue
class Solution:
    def averageOfLevels(self, root):
        """
        :type root: TreeNode
        :rtype: List[float]
        """
        
        if root is None:
            return [];

        row = [root]
        result = []
        while any(row):
            result.append(sum([v.val for v in row]) / len(row))
            row = [sub for node in row for sub in (node.left, node.right) if sub]
        return result
        
```



