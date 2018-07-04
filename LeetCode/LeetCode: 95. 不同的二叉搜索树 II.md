# LeetCode: 95. 不同的二叉搜索树 II

[TOC]

## 1、题目描述



给定一个整数 *n*，生成所有由 1 ... *n* 为节点所组成的**二叉搜索树**。

**示例:**

```
输入: 3
输出:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
解释:
以上的输出对应以下 5 种不同结构的二叉搜索树：

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```



## 2、解题思路

​	看到二叉树，首先想到的就是递归求解，不顾哦这个形式有一点特殊，特殊在哪里呢？



​	这个题目是典型的分治法，但是有一点不同，就在于，小问题之间需要相互组合求解，例如，确定一个根节点，但是左节点的可能性有多个，右节点的可能性也有多个，于是，同一个根节点，就有了几棵树



​	基本的解题思路是这样的

- 从1到n中，先确定根节点
- 假如确定了5，那么1-4中是他的左子树，6-n是右子树
- 左子树和右子树都有多个，这时候，相互之间的组合就有多个





```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def generateTrees(self, n):
        """
        :type n: int
        :rtype: List[TreeNode]
        """
        if n <= 0:
            return []

        return self.dfs(1, n)

    def dfs(self, left, right):
        result = []

        if left > right:
            result.append(None)
            return result

        for i in range(left, right + 1):
            for left_node in self.dfs(left, i - 1):
                for right_node in self.dfs(i + 1, right):
                    root_node = TreeNode(i)
                    root_node.left = left_node
                    root_node.right = right_node
                    result.append(root_node)

        return result
```

