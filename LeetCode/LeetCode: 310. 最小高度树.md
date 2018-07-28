# LeetCode: 310. 最小高度树

[TOC]

## 1、题目描述

对于一个具有树特征的无向图，我们可选择任何一个节点作为根。图因此可以成为树，在所有可能的树中，具有最小高度的树被称为最小高度树。给出这样的一个图，写出一个函数找到所有的最小高度树并返回他们的根节点。

**格式**

该图包含 `n` 个节点，标记为 `0` 到 `n - 1`。给定数字 `n` 和一个无向边 `edges` 列表（每一个边都是一对标签）。

你可以假设没有重复的边会出现在 `edges` 中。由于所有的边都是无向边， `[0, 1]`和 `[1, 0]` 是相同的，因此不会同时出现在 `edges` 里。

**示例 1:**

```
输入: n = 4, edges = [[1, 0], [1, 2], [1, 3]]

        0
        |
        1
       / \
      2   3 

输出: [1]
```

**示例 2:**

```
输入: n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

     0  1  2
      \ | /
        3
        |
        4
        |
        5 

输出: [3, 4]
```

**说明**:

-  根据[树的定义](https://baike.baidu.com/item/%E6%A0%91/2699484?fromtitle=%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84+%E6%A0%91&fromid=12062173&fr=aladdin)，树是一个无向图，其中任何两个顶点只通过一条路径连接。 换句话说，一个任何没有简单环路的连通图都是一棵树。
- 树的高度是指根节点和叶子节点之间最长向下路径上边的数量。



## 2、解题思路

​	因为这个题目是树，实际上也就是无向图，每个节点都是有度的，叶子节点的度是1，非叶子节点的度是2

​	然后我们将所有节点的度找出来，首先将所有度为1的节点，放到队列中，然后与这个节点有边相连接的节点，度会减少1

​	返回的结果，就是这些减少了一个度以后，度变成1的节点

​	实际上是一个广度优先搜索，从最底层开始，也就是叶子节点



​	一开始把题意理解错了，一位是找到叶子节点的子节点，实际上，是整棵树的根节点

```
        0
        | 
        1
        |
        2
        |
        3 
```

如果是上面的情况，我们想要得到度最小的树，就是以1，2为根节点的树，返回这两个节点



设计两个list，一个保存节点的度，一个保存与节点相连接的节点

使用层次遍历，用一个缓冲

```python
import queue
class Solution:
    def findMinHeightTrees(self, n, edges):
        """
        :type n: int
        :type edges: List[List[int]]
        :rtype: List[int]
        """
        
        if n == 1:
            return [0]

        if len(edges) == 0:
            return [i for i in range(n)]

        degree = [0] * n
        pre_node = [[] for _ in range(n)]

        q = queue.deque()

        for i in edges:
            pre_node[i[0]] += [i[1]]
            pre_node[i[1]] += [i[0]]

            degree[i[0]] += 1
            degree[i[1]] += 1

        for i, v in enumerate(degree):
            if v == 1:
                q.append(i)
                degree[i] = 0
        result = []

        while q:
            size = len(q)
            result = list(q)
            for i in range(size):
                node = q.popleft()
                for pre in pre_node[node]:
                    degree[pre] -= 1

                    if degree[pre] == 1:
                        q.append(pre)
                degree[node] = 0
        return result
```

​	嘿嘿嘿，战胜100%。。。。。。

​	