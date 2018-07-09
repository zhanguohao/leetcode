# LeetCode: 173. 二叉搜索树迭代器

[TOC]

## 1、题目描述

实现一个二叉搜索树迭代器。你将使用二叉搜索树的根节点初始化迭代器。

调用 `next()` 将返回二叉搜索树中的下一个最小的数。

注意: `next()` 和`hasNext()` 操作的时间复杂度是O(1)，并使用 *O(h)* 内存，其中 *h* 是树的高度。



## 2、解题思路

​	想要得到下一个数值，我们使用栈来存储，因为要得到最小值，所以是中遍历，放到栈里面，顺序相反，因此遍历的顺序应该是右-》根 -》左

​	这样就能够不断地pop了

```python
# Definition for a  binary tree node
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator(object):
    
    def __init__(self, root):
        """
        :type root: TreeNode
        """
        self.buff = []
        self.dfs(root)
        print(self.buff)

    def hasNext(self):
        """
        :rtype: bool
        """
        if self.buff:
            return True

    def next(self):
        """
        :rtype: int
        """
        return self.buff.pop()

    def dfs(self, node):
        if not node:
            return
        self.dfs(node.right)
        self.buff.append(node.val)
        self.dfs(node.left)
        

# Your BSTIterator will be called like this:
# i, v = BSTIterator(root), []
# while i.hasNext(): v.append(i.next())
```



​	或者说另一个思路，不必一次缓冲所有的，直接按照非递归中序遍历，缓存前面的节点即可

