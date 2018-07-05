# LeetCode: 501. 二叉搜索树中的众数

[TOC]

## 1、题目描述





Given a binary search tree (BST) with duplicates, find all the [mode(s)](https://en.wikipedia.org/wiki/Mode_(statistics)) (the most frequently occurred element) in the given BST.

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
- The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
- Both the left and right subtrees must also be binary search trees.

For example:
Given BST `[1,null,2,2]`,

```
   1
    \
     2
    /
   2
```

return `[2]`.

**Note:** If a tree has more than one mode, you can return them in any order.

**Follow up:** Could you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).





给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

假定 BST 有如下定义：

- 结点左子树中所含结点的值小于等于当前结点的值
- 结点右子树中所含结点的值大于等于当前结点的值
- 左子树和右子树都是二叉搜索树

例如：
给定 BST `[1,null,2,2]`,

```
   1
    \
     2
    /
   2
```

`返回[2]`.

**提示**：如果众数超过1个，不需考虑输出顺序

**进阶：**你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）





## 2、解题思路

​	首先，假设我们有一个排好序的数组，从前往后寻找出现次数最多的那个数，如果遇到一个，就来判断一下，当前数字的次数，如果等于最大计数值，就加入到结果数组中，如果大于，就将结果数组清空，并加入当前数

​	因为是二叉搜索树，因此，中序遍历就得到了

```c
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    
    current_value = 0;
    max_count = 0;
    count = 0;
    result = [];
    
    def findMode(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """

        if root != None:
            current_value = root.val
        else:
            return [];

        self.find(root)

        if self.count > self.max_count:
            self.result.clear()
            self.result.append(self.current_value) 
        elif self.count == self.max_count:
            self.result.append(self.current_value) 

        return self.result;


        
    def find(self,root):
        if root == None:
            return ;
        self.find(root.left)
        if root.val == self.current_value:
            self.count += 1

        else:

            if self.count > self.max_count:
                self.result.clear()
                self.result.append(self.current_value) 
                self.max_count = self.count
            elif self.count == self.max_count:
                self.result.append(self.current_value) 
            self.count = 1;
            self.current_value = root.val

        self.find(root.right)
```







