# LeetCode: 222. 完全二叉树的节点个数

[TOC]



## 1、题目描述



给出一个**完全二叉树**，求出该树的节点个数。

**说明：**

[完全二叉树](https://baike.baidu.com/item/%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91/7773232?fr=aladdin)的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

**示例:**

```
输入: 
    1
   / \
  2   3
 / \  /
4  5 6

输出: 6
```



## 2、解题思路

​	判断当前的二叉树是不是完全二叉树，如果是，返回直接计算出来，返回

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def countNodes(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0

        left = self.getLeftDepth(root)
        right = self.getrightDepth(root)
        if left == right:
            return 2 ** left - 1
        else:
            return 1 + self.countNodes(root.left) + self.countNodes(root.right)

    def getLeftDepth(self, root):
        count = 1
        while root.left:
            count += 1
            root = root.left

        return count

    def getrightDepth(self, root):
        count = 1
        while root.right:
            count += 1
            root = root.right

        return count
```

