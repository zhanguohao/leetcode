# LeetCode: 449. 序列化和反序列化二叉搜索树

[TOC]

## 1、题目描述

序列化是将数据结构或对象转换为一系列位的过程，以便它可以存储在文件或内存缓冲区中，或通过网络连接链路传输，以便稍后在同一个或另一个计算机环境中重建。

设计一个算法来序列化和反序列化**二叉搜索树**。 对序列化/反序列化算法的工作方式没有限制。 您只需确保二叉搜索树可以序列化为字符串，并且可以将该字符串反序列化为最初的二叉搜索树。

**编码的字符串应尽可能紧凑。**

**注意**：不要使用类成员/全局/静态变量来存储状态。 你的序列化和反序列化算法应该是无状态的。

## 2、解题思路

​	序列化的时候，我们可以选择使用前序遍历来做

​	反序列化，同样的，根据前序遍历，我们使用递归来做

反序列化：

- 使用双向队列存储所有节点
- 因为是二叉搜索树，因此当前节点的左节点是小于当前节点的值，右节点是大于当前节点的值



```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from collections import deque
class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        
        ans = []

        def pre_order(node):
            if not node:
                return
            ans.append(node.val)
            pre_order(node.left)
            pre_order(node.right)

        pre_order(root)
        return ' '.join(map(str, ans))

    def deserialize(self, data):
        """Decodes your encoded data to tree.

        :type data: str
        :rtype: TreeNode
        """
        if not data:
            return None
        q = deque(map(int, data.split(" ")))

        def dfs(minVal, maxVal):
            if q and minVal < q[0] < maxVal:
                val = q.popleft()
                node = TreeNode(val)
                node.left = dfs(minVal, val)
                node.right = dfs(val, maxVal)
                return node

        return dfs(-sys.maxsize, sys.maxsize)
        

# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))
```

