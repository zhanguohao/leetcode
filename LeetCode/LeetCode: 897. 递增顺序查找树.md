# LeetCode: 897. 递增顺序查找树

[TOC]

## 1、题目描述

给定一个树，**按顺序**重新排列树，使树中最左边的结点现在是树的根，并且每个结点没有左子结点，只有一个右子结点。

 

**示例 ：**

```
输入：[5,3,6,2,4,null,8,1,null,null,null,7,9]

       5
      / \
    3    6
   / \    \
  2   4    8
 /        / \ 
1        7   9

输出：[1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

 1
  \
   2
    \
     3
      \
       4
        \
         5
          \
           6
            \
             7
              \
               8
                \
                 9  
```

 

**提示：**

1. 给定树中的结点数介于 1 和 100 之间。
2. 每个结点都有一个从 0 到 1000 范围内的唯一整数值。



## 2、解题思路

	这道题直接使用中序遍历，就能得到有序

	然后设置一个指针，只需要不断的将当前节点的左节点设置为空，右节点指向下一个，然后指针后移

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def increasingBST(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        res = current = TreeNode(0)

        def dfs(node):
            nonlocal current
            if not node:
                return
            dfs(node.left)

            node.left = None
            current.right = node
            current = current.right
            dfs(node.right)
        dfs(root)
        return res.right
```

