# LeetCode: 103. 二叉树的锯齿形层次遍历

[TOC]



## 1、题目描述

​	给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```

## 2、解题思路

​	与前面102题基本差不多，只需要加一个控制变量，不断地将每一行的元素反转，放到结果数组中即可



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

import queue

class Solution:
    def zigzagLevelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        result = []
        q = queue.Queue()
        reverse_list = False
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
            if not reverse_list:
                result.append(temp)
            else:
                result.append(list(reversed(temp)))
            reverse_list = not reverse_list
            

        return result
```

