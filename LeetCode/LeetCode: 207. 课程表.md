# LeetCode: 207. 课程表

[TOC]



## 1、题目描述



现在你总共有 *n* 门课需要选，记为 `0` 到 `n-1`。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: `[0,1]`

给定课程总量以及它们的先决条件，判断是否可能完成所有课程的学习？

**示例 1:**

```
输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
```

**示例 2:**

```
输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
```

**说明:**

1. 输入的先决条件是由**边缘列表**表示的图形，而不是邻接矩阵。详情请参见[图的表示法](http://blog.csdn.net/woaidapaopao/article/details/51732947)。
2. 你可以假定输入的先决条件中没有重复的边。

**提示:**

1. 这个问题相当于查找一个循环是否存在于有向图中。如果存在循环，则不存在拓扑排序，因此不可能选取所有课程进行学习。
2. [通过 DFS 进行拓扑排序](https://www.coursera.org/specializations/algorithms) - 一个关于Coursera的精彩视频教程（21分钟），介绍拓扑排序的基本概念。
3. 拓扑排序也可以通过 [BFS](https://baike.baidu.com/item/%E5%AE%BD%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2/5224802?fr=aladdin&fromid=2148012&fromtitle=%E5%B9%BF%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2) 完成。



## 2、解题思路

​	首先回复一下图的基本知识，如何表示一个图呢

1、邻接矩阵

2、邻接表



​	具体的表示方法可以参考相关书籍

一般利用深度优先搜索和广度优先搜索来做

​	我们将这个图的拓扑序检测出来，如果存在拓扑序，表示能够学完所有课程，否则不能

​	dfs和bfs都是根据节点的度来做的，bfs是入度，dfs是出度

​	

- 使用dfs

基本的解题思路就是，首先表示出这个图，并且将每个节点的出度计算出来

将所有的出度为0的节点当度放到队列中，然后将他的前面节点的出度减一，如果出度减到0，放到队列中

设置一个计数器，每一次将出度为0的弹出，就将计数器加一

如果队列为空，表示已经遍历完成

这时候，判断计数器的值与所有课程是不是相等，如果相等，表示拓扑序存在，也就能够完成所有课程

​	

​	使用两个字典，一个字典存放每个节点度的值，另一个字典存放每个节点的前向节点列表，如果把这个出度为0的点移除，就将他的前驱节点的入度减一

```python
import queue

class Solution:
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        degree = {}
        pre_class = {}

        for i in prerequisites:
            if pre_class.get(i[0]) is None:
                pre_class[i[0]] = [i[1]]
            else:
                pre_class[i[0]] += [i[1]]

            if degree.get(i[1]) is None:
                degree[i[1]] = 1
            else:
                degree[i[1]] += 1

        d = queue.deque()

        for i in range(numCourses):
            if i not in degree:
                d.append(i)

        count = 0
        while d:
            size = len(d)
            for i in range(size):
                node = d.pop()
                count += 1
                if pre_class.get(node) is not None:
                    for j in pre_class[node]:
                        degree[j] -= 1
                        if degree[j] == 0:
                            d.append(j)
       
        if count == numCourses:
            return True
        else:
            return False
```

