# LeetCode: 144. 二叉树的前序遍历

[TOC]



## 1、题目描述



给定一个二叉树，返回它的 *前序* 遍历。

 **示例:**

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
```



## 2、解题思路

### 2.1 递归法

​	递归法已经做了很多遍了，直接写出来就好

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):

    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        
        
        if not root:
            return []

        result = []
        self.dfs(root, result)
        return result

    def dfs(self, node, result):
        if node:
            result.append(node.val)
        else:
            return
        self.dfs(node.left, result)
        self.dfs(node.right, result)
```



### 2.2 迭代法

​	迭代法借助队列来做

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):

    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        
        if not root:
            return []

        result = []

        stack = [root]
        while stack:
            node = stack.pop()
            result.append(node.val)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        return result
```

