# LeetCode: 866. 具有所有最深结点的最小子树

[TOC]



## 1、题目描述



给定一个根为 `root` 的二叉树，每个结点的*深度*是它到根的最短距离。

如果结点具有最大深度，则该结点是*最深的*。

返回具有最大深度的结点，以该结点为根的子树中包含所有最深的结点。

 

**示例：**

```
输入：[3,5,1,6,2,0,8,null,null,7,4]
输出：[2,7,4]
解释：



我们返回值为 2 的结点，在图中用黄色标记。
在图中用蓝色标记的是树的最深的结点。
输入 "[3, 5, 1, 6, 2, 0, 8, null, null, 7, 4]" 是对给定的树的序列化表述。
输出 "[2, 7, 4]" 是对根结点的值为 2 的子树的序列化表述。
输入和输出都具有 TreeNode 类型。
```

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png) 



**提示：**

- 树中结点的数量介于 1 和 500 之间。



## 2、解题思路

​	这道题一开始没有理解思路，就是如果只有一个深度最深的节点，直接返回就好了

​	如果有多个，那么那个节点的子树能够包含这两个，就返回这个节点

​	当然，根节点肯定是包含的

​	可以这样分析，如果左子树的深度等于右子树的深度，就返回根节点

​	如果左子树的深度大于右子树的深度，抛弃右子树，重新判断左子树即可

​	最终得到的就是左右子树深度一致的节点，并且包含最深的节点

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:

    def subtreeWithAllDeepest(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        
        if not root:
            return root

        if self.dfs(root.left) == self.dfs(root.right):
            return root
        elif self.dfs(root.left) > self.dfs(root.right):
            return self.subtreeWithAllDeepest(root.left)
        else:
            return self.subtreeWithAllDeepest(root.right)

    def dfs(self, node):
        if not node:
            return 0
        return max(self.dfs(node.left) + 1, self.dfs(node.right) + 1)
```

