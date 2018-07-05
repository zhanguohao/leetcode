# LeetCode: 106. 从中序与后序遍历序列构造二叉树

[TOC]



## 1、题目描述



根据一棵树的中序遍历与后序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```



## 2、解题思路

​	在前面105题中，使用前序遍历和中序遍历得到二叉树一样，后序遍历就是根节点放在了最后，其他的与前面没有区别



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def buildTree(self, inorder, postorder):
        """
        :type inorder: List[int]
        :type postorder: List[int]
        :rtype: TreeNode
        """
        if len(inorder) <= 0:
            return None

        root = TreeNode(postorder[-1])

        root_index = inorder.index(postorder[-1])

        inorder_left_subTree = inorder[:root_index]
        inorder_right_subTree = inorder[root_index + 1:]

        postorder_left_subTree = postorder[:len(inorder_left_subTree)]
        postorder_right_subTree = postorder[len(inorder_left_subTree):-1]

        root.left = self.buildTree(inorder_left_subTree, postorder_left_subTree)
        root.right = self.buildTree(inorder_right_subTree, postorder_right_subTree)

        return root
```





