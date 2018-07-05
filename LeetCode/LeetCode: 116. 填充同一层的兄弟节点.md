# LeetCode: 116. 填充同一层的兄弟节点

[TOC]



## 1、题目描述

​	给定一个二叉树

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
- 你可以假设它是一个完美二叉树（即所有叶子节点都在同一层，每个父节点都有两个子节点）。

**示例:**

给定完美二叉树，

```
     1
   /  \
  2    3
 / \  / \
4  5  6  7
```

调用你的函数后，该完美二叉树变为：

```
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \  / \
4->5->6->7 -> NULL
```



## 2、解题思路

### 2.1 递归求解

​	首先，因为是完美二叉树，那么下一行的判断就能够依据上一行的结果

- 给定一个节点，如果当前节点有左子树，右子树，就将左子树指向右子树，右子树指向父节点的兄弟节点的左子树



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
        if not root.left:
            return
        root.left.next = root.right
        if root.next:
            root.right.next = root.next.left
        self.connect(root.left)
        self.connect(root.right)
```



### 2.2 非递归法

​	实际上，不一定要用递归来做，也可以通过遍历每一行来做，也是借助上一行的结果，设置两个指针，一个指向最左端，从最左端移动到最右端

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
        if not root.left:
            return
        line_start = root
        while line_start.left:
            cur = line_start
            while cur:
                cur.left.next = cur.right
                if cur.next:
                    cur.right.next = cur.next.left

                cur = cur.next

            line_start = line_start.left
```





