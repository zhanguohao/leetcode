# LeetCode: 109. 有序链表转换二叉搜索树

[TOC]



## 1、题目描述

​	给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。

**示例:**

```
给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```





## 2、解题思路

​	在前面，108题中，是数组的形式，每一次找到中间值，然后分成左右两部分，递归操作

​	但是在数组中，找到中间值很简单，在链表中，找到中间值可以利用两个指针，一个快，一个慢，块的每一次走两步，慢的走一步，这样慢的指针就是想要找的中间值

​	一开始想到的是利用从头开始走的，不过超出了时间限制

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sortedListToBST(self, head):
        """
        :type head: ListNode
        :rtype: TreeNode
        """
        
        left = 0
        right = self.getLength(head) - 1

        return self.dfs(head, left, right)

    def dfs(self, root, left, right):
        if left > right:
            return None

        mid = (left + right+1) // 2
        mid_temp = mid
        temp = root
        mid_val = root.val
        while mid_temp > 0:
            temp = temp.next
            mid_val = temp.val
            mid_temp -= 1

        result = TreeNode(mid_val)

        result.left = self.dfs(root, left, mid - 1)
        result.right = self.dfs(root, mid + 1, right)

        return result

    def getLength(self, root):
        if not root:
            return 0
        count = 0
        while root:
            count += 1
            root = root.next

        return count
```



​	利用双指针求中间值

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sortedListToBST(self, head):
        """
        :type head: ListNode
        :rtype: TreeNode
        """
        if not head:
            return None

        return self.dfs(head, None)

    def dfs(self, head, tail):
        if head == tail:
            return None

        faster = head
        slower = head
        while faster != tail:
            faster = faster.next
            if faster != tail:
                faster = faster.next
                slower = slower.next

        node = TreeNode(slower.val)
        node.left = self.dfs(head, slower)
        node.right = self.dfs(slower.next, tail)

        return node
```

