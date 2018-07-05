# LeetCode: 114. 二叉树展开为链表

[TOC]



## 1、题目描述



给定一个二叉树，[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95/8010757)将它展开为链表。

例如，给定二叉树

```
    1
   / \
  2   5
 / \   \
3   4   6
```

将其展开为：

```
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
```



## 2、解题思路

### 2.1 递归法

​	一般看到二叉树，第一个想到的就是递归求解

​	首先，我们来分析一下将二叉树展开成链表的规律

- 对根节点来讲，如果左子树和右子树都展开了，直接将左子树中最右端的端点的右指针指向当前的右子树，左子树变成右子树，左子树变成None即可

- 对每个节点来讲，都是如此

  那么使用什么方式，将左右子树变成链表呢？

  首先我们确定，左右子树应该是已经变成链表以后进行操作，所以，应该是后续遍历

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def flatten(self, root):
        """
        :type root: TreeNode
        :rtype: void Do not return anything, modify root in-place instead.
        """
        if not root:
            return None
        self.flatten(root.left)
        self.flatten(root.right)
        if root.left:
            temp = root.right
            left_node = root.left
            while left_node.right:
                left_node = left_node.right
            root.right = root.left
            root.left = None
            left_node.right = temp
```



### 2.2 非递归法

​	实际上还可以用非递归法，比如说我们知道，当前节点的右子树应该放在什么位置呢？

​	就是当前节点左子树的最右节点的后面，我们直接把它放过去，然后，指针后移，如果有左子树，就把右子树放到左子树的最右面节点的后面

​	以此类推，直到最后一个节点，所有的就都被变成了链表了

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def flatten(self, root):
        """
        :type root: TreeNode
        :rtype: void Do not return anything, modify root in-place instead.
        """
        
        if not root:
            return None

        while root:
            if root.left:
                temp = root.right
                left_node = root.left
                while left_node.right:
                    left_node = left_node.right
                root.right = root.left
                root.left = None
                left_node.right = temp
            root = root.right
```





