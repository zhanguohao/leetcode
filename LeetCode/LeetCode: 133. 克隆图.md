# LeetCode: 133. 克隆图

[TOC]



## 1、题目描述



克隆一张无向图，图中的每个节点包含一个 `label` （标签）和一个 `neighbors` （邻接点）列表 。

**OJ的无向图序列化：**

节点被唯一标记。

我们用 `#` 作为每个节点的分隔符，用 `,` 作为节点标签和邻接点的分隔符。

例如，序列化无向图 `{0,1,2#1,2#2,2}`。

该图总共有三个节点, 被两个分隔符  `#` 分为三部分。 

1. 第一个节点的标签为 `0`，存在从节点 `0` 到节点 `1` 和节点 `2` 的两条边。
2. 第二个节点的标签为 `1`，存在从节点 `1` 到节点 `2` 的一条边。
3. 第三个节点的标签为 `2`，存在从节点 `2` 到节点 `2` (本身) 的一条边，从而形成自环。

我们将图形可视化如下：

```
       1
      / \
     /   \
    0 --- 2
         / \
         \_/
```



## 2、解题思路

​	看到图的问题，首先想到的就是深度优先很广度优先

- 使用深度优先

  ​	一开始写的深度优先，没有考虑到自循环的问题，陷入自循环中，所以，应该标记一下，已经访问过的点，保存到字典中，如果是已经添加过的节点，直接放到邻居中即可，不需要重新的生成一个节点

  ​	

  ```python
  # Definition for a undirected graph node
  # class UndirectedGraphNode:
  #     def __init__(self, x):
  #         self.label = x
  #         self.neighbors = []
  
  class Solution:
      
      node_dict = {}
      # @param node, a undirected graph node
      # @return a undirected graph node
      def cloneGraph(self, node):
          
          if not node:
              return None
  
          return self.dfs(node)
  
      def dfs(self, node):
          if node in self.node_dict:
              return self.node_dict[node]
  
          current = UndirectedGraphNode(node.label)
          self.node_dict[node] = current
          for i in node.neighbors:
              self.node_dict[node].neighbors += [self.dfs(i)]
          return self.node_dict[node]
  ```

  