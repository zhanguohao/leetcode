# LeetCode: 98. 验证二叉搜索树

[TOC]



## 1、题目描述



给定一个二叉树，判断其是否是一个有效的二叉搜索树。

一个二叉搜索树具有如下特征：

- 节点的左子树只包含**小于**当前节点的数。
- 节点的右子树只包含**大于**当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1:**

```
输入:
    2
   / \
  1   3
输出: true
```

**示例 2:**

```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```





## 2、解题思路

​	假设左子树是二叉搜索树，右子树也是二叉搜索树，那么只要当前节点大于左子树的最大节点，小于右子树的最小节点即可

​	其实一个简单的做法，二叉搜索树的中序遍历是一个排序数组，如果使用中序遍历找到最后，每一个值都是大于前面的值，表示这就是二叉搜索树

​	

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    
    prev = -1
    res = True
    
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        self.dfs(root)
        return self.res

    def dfs(self, node):
        if node is not None:
            self.dfs(node.left)
            if self.prev != -1:
                if node.val <= self.prev:
                    self.res = False
                    return
            self.prev = node.val
            self.dfs(node.right)
```

​	需要注意的是，prev和res不能采用参数传递的方式，因为每个函数有自己的调用栈，会导致每次使用的都是之前的prev

​	



