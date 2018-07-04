# LeetCode: 94. 二叉树的中序遍历

[TOC]



## 1、题目描述



给定一个二叉树，返回它的*中序* 遍历。

**示例:**

```
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？



## 2、解题思路

​	一般二叉树的问题都能够用递归来解决，不过这个问题既然不用递归，就需要借助数据结构，例如栈来存储

前面尚未放到结果中的节点



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
import queue

class Solution:
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        result = []

        q = queue.deque()
        if root:
            q.append(root)

        judge = True

        while q:

            if judge and q[-1].left:
                q.append(q[-1].left)
                continue
            judge = False
            result.append(q[-1].val)

            if q[-1].right:
                q.append(q.pop().right)
                judge = True
            else:
                q.pop()
        return result
```

​	一开始，没有用judge变量，发现陷入了死循环，然后分析了一下，发现，是因为我们将队列中的节点弹出，这时候就不应该是去判断左节点，不然那就会出现下面的情况

- 弹出左节点
- 发现有左节点，放入

这种情况是不对的

​	什么时候重新判断左节点呢？

就是当放入新的右节点的时候，需要判断他有没有左节点

