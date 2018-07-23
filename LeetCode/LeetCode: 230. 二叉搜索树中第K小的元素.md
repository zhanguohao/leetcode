# LeetCode: 230. 二叉搜索树中第K小的元素

[TOC]



## 1、题目描述

给定一个二叉搜索树，编写一个函数 `kthSmallest` 来查找其中第 **k** 个最小的元素。

**说明：**
你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。

**示例 1:**

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 1
```

**示例 2:**

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 3
```

**进阶：**
如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第 k 小的值，你将如何优化 `kthSmallest` 函数？

## 2、解题思路

### 2.1 中序遍历

​	直接使用中序遍历，进行计数，找出第k个元素

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    
    count = 0
    result = 0

    def kthSmallest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """

        self.dfs(root, k)
        self.count = 0
        return self.result

    def dfs(self, root, k):
        if not root:
            return
        self.dfs(root.left, k)
        self.count += 1
        if self.count == k:
            self.result = root.val
            return
        self.dfs(root.right, k)
```

