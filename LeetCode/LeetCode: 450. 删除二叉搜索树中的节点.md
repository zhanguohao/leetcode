# LeetCode: 450. 删除二叉搜索树中的节点

[TOC]

## 1、题目描述

给定一个二叉搜索树的根节点 **root** 和一个值 **key**，删除二叉搜索树中的 **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

**说明：** 要求算法时间复杂度为 O(h)，h 为树的高度。

**示例:**

```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。

一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。

    5
   / \
  4   6
 /     \
2       7

另一个正确答案是 [5,2,6,null,4,null,7]。

    5
   / \
  2   6
   \   \
    4   7
```

## 2、解题思路

​	分几种情况讨论

- 首先寻找当前节点，如果找到，删除节点，并调整树
- 调整树:
  - 如果存在右子树，使用右子节点替换当前节点，并判断是否存在左节点，存在的话，右节点的进行找到树中最左节点，将左子树挂在上面
  - 如果不存在右子树，当前节点的位置直接由左节点替换

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def deleteNode(self, root, key):
        """
        :type root: TreeNode
        :type key: int
        :rtype: TreeNode
        """
        
        ans = father = TreeNode(0)
        father.right = root
        cur = root

        def search(pre, current, target):

            while current:
                if current.val == target:
                    return pre, current
                pre = current
                if current.val > target:
                    current = current.left
                else:
                    current = current.right

            return None, None

        def adjust(pre, current):

            if not current:
                return

            direction = 0
            if pre.right is current:
                direction = 1

            if not current.left:
                if not direction:
                    pre.left = current.right
                else:
                    pre.right = current.right
                return
            if not current.right:
                if not direction:
                    pre.left = current.left
                else:
                    pre.right = current.left
                return

            if not direction:
                pre.left = current.right
            else:
                pre.right = current.right

            temp = current.right

            while temp.left:
                temp = temp.left
            temp.left = current.left
            return
        adjust(*(search(father, cur, key)))
        return ans.right
```

​	效率还是不错的，超过98左右，不过后面的条件判断有些冗余，以后再来改，不过思路理解很简单