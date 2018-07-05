# LeetCode: 113. 路径总和 II

[TOC]



## 1、题目描述



给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

**说明:** 叶子节点是指没有子节点的节点。

**示例:**
给定如下二叉树，以及目标和 `sum = 22`，

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

返回:

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```



## 2、解题思路

​	看到这道题，直接想到用dfs来做

​	每次传入当前节点的想要求解的和，然后减去当前节点的值，判断，当前节点是不是叶子节点，然后如果是，并且和是0，放入结果数组

​	



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: List[List[int]]
        """
        if not root:
            return []
        result = []
        self.dfs(root, sum, [], result)
        return result

    def dfs(self, root, cur_sum, current, result):
        temp_sum = cur_sum - root.val
        if not root.left and not root.right and temp_sum == 0:
            current.append(root.val)
            result.append(current)
            return

        if root.left:
            left_temp = current[:] + [root.val]
            self.dfs(root.left, temp_sum, left_temp, result)

        if root.right:
            right_temp = current[:] + [root.val]
            self.dfs(root.right, temp_sum, right_temp, result)
```

