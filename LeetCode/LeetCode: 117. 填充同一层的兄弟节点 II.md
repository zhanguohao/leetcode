# LeetCode: 117. 填充同一层的兄弟节点 II

[TOC]



## 1、题目描述



给定一个二叉树

```
struct TreeLinkNode {
  TreeLinkNode *left;
  TreeLinkNode *right;
  TreeLinkNode *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`。

**说明:**

- 你只能使用额外常数空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

**示例:**

给定二叉树，

```
     1
   /  \
  2    3
 / \    \
4   5    7
```

调用你的函数后，该二叉树变为：

```
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \    \
4-> 5 -> 7 -> NULL
```



## 2、解题思路

​	这道题和前面116的区别就在于这个二叉树不是完美二叉树，可能是没有左子树，或者没有右子树

​	所以，每一行想要找到第一个元素很关键，所以，我们创建一个节点，这个节点指向每一行的第一个元素

​	然后每一行修改完毕以后，移动到下一行

​	

```python
# Definition for binary tree with next pointer.
# class TreeLinkNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#         self.next = None

class Solution:
    # @param root, a tree link node
    # @return nothing
    def connect(self, root):
        
        if not root:
            return
        if not root.left and not root.right:
            return

        first_node = TreeLinkNode(0)
        left = first_node
        while root:
            if root.left:
                left.next = root.left
                left = root.left

            if root.right:
                left.next = root.right
                left = root.right

            root = root.next
            if not root:
                left = first_node
                root = first_node.next
                first_node.next = None
```



