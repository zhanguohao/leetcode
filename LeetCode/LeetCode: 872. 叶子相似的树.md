# LeetCode: 872. 叶子相似的树

[TOC]



## 1、题目描述



考虑一个二叉树的所有叶子。这些叶子的值按从左到右的顺序排列形成一个 *叶值序列* 。

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/16/tree.png)

举个例子，给定一个如上图所示的树，其叶值序列为 `(6, 7, 4, 9, 8)` 。

如果两个二叉树的叶值序列相同，我们就认为它们是 *叶相似的*。

如果给定的两个头结点分别为 `root1` 和 `root2` 的树是叶相似的，返回 `true`；否则返回 `false` 。

 

**提示：**

- 给定的两个树会有 `1` 到 `100` 个结点。





## 2、解题思路

​	使用深度优先搜索，判断如果当前节点是叶子节点，节加入到结果数组中

- 如果结果数组长度一致，继续判断，不一致返回False



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def leafSimilar(self, root1, root2):
        """
        :type root1: TreeNode
        :type root2: TreeNode
        :rtype: bool
        """
        self.buff = []
        self.dfs(root1)
        temp1 = self.buff.copy()
        self.buff = []
        self.dfs(root2)

        if len(temp1) != len(self.buff):
            return False
        else:
            for i in range(len(temp1)):
                if temp1[i] != self.buff[i]:
                    return False

        return True

    def dfs(self, root):
        if not root:
            return
        if not root.left and not root.right:
            self.buff.append(root.val)
        self.dfs(root.left)
        self.dfs(root.right)
```

