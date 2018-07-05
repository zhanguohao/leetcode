# LeetCode: 105. 从前序与中序遍历序列构造二叉树

[TOC]



## 1、题目描述



根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
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

​	根据前序遍历，我们直接能够找到当前的根节点是哪个

​	然后找到在中序遍历中的位置，左面的就是左子树的中序遍历结果，右面的就是右子树的遍历结果

​	这样我们就能进行递归了，生成一个根节点，然后将左子树的前序遍历，中序遍历递归，得到了左子树；右子树的前序遍历结果与中序遍历结果递归，得到右子树

​	

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if len(preorder) <= 0:
            return None

        root = TreeNode(preorder[0])

        root_index = inorder.index(preorder[0])

        inorder_left_subTree = inorder[:root_index]
        inorder_right_subTree = inorder[root_index + 1:]

        preorder_left_subTree = preorder[1:len(inorder_left_subTree)+1]
        preorder_right_subTree = preorder[len(inorder_left_subTree)+1:]

        root.left = self.buildTree(preorder_left_subTree, inorder_left_subTree)
        root.right = self.buildTree(preorder_right_subTree, inorder_right_subTree)

        return root
```



​	