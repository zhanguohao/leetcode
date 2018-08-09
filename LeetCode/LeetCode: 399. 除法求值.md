# LeetCode: 399. 除法求值

[TOC]

## 1、题目描述

给出方程式 `A / B = k`, 其中 `A` 和 `B` 均为代表字符串的变量， `k` 是一个浮点型数字。根据已知方程式求解问题，并返回计算结果。如果结果不存在，则返回 `-1.0`。

**示例 :**
给定 `a / b = 2.0, b / c = 3.0`
问题: `a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? `
返回 `[6.0, 0.5, -1.0, 1.0, -1.0 ]`

输入为: `vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries`(方程式，方程式结果，问题方程式)， 其中 `equations.size() == values.size()`，即方程式的长度与方程式结果长度相等（程式与结果一一对应），并且结果值均为正数。以上为方程式的描述。 返回`vector<double>`类型。

基于上述例子，输入如下：

```
equations(方程式) = [ ["a", "b"], ["b", "c"] ],
values(方程式结果) = [2.0, 3.0],
queries(问题方程式) = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```

输入总是有效的。你可以假设除法运算中不会出现除数为0的情况，且不存在任何矛盾的结果。

## 2、解题思路

​	这道题是一个图相关的，是一个有向图，将每一个变量看成是节点，节点之间的边是前面节点除以后面节点的值，而我们要找的答案，就是遍历这个图，找到问题方程式所经过的路径，并且将所有的路径值乘起来，就得到了答案；

​	该如何存储相互之间的关系呢？

​	在python里面，直接使用字典即可，键就表示前面的节点，值的话，需要保存两个，一个是后面的一个节点，另一个是边的值，如下所示

```
[ ["a", "b"], ["b", "c"] ]
[2.0, 3.0]
我们对上面的进行转换，
defaultdict(<class 'dict'>, {'a': {'a': 1.0, 'b': 2.0}, 'c': {'b': 0.3333333333333333, 'c': 1.0}, 'b': {'a': 0.5, 'c': 3.0, 'b': 1.0}})
找出每一个值能够直接除以得到的结果
```

​	然后使用深度优先搜索，找出来即可

```python
class Solution:
    def calcEquation(self, equations, values, queries):
        """
        :type equations: List[List[str]]
        :type values: List[float]
        :type queries: List[List[str]]
        :rtype: List[float]
        """
        graph = collections.defaultdict(dict)
        res = [0] * len(queries)

        for (x, y), val in zip(equations, values):
            graph[x][x] = graph[y][y] = 1.0
            graph[x][y] = val
            graph[y][x] = 1 / val
            
        for i in range(len(queries)):
            temp_graph = graph.copy()
            res[i] = self.dfs(queries[i][0], queries[i][1], temp_graph)

        return res

    def dfs(self, x, y, graph):
        if x in graph:
            if y in graph[x]:
                return graph[x][y]
            else:
                temp = graph[x]
                graph.pop(x)
                for i in temp:
                    if i != x:
                        val = self.dfs(i, y, graph)
                        if val > 0:
                            return temp[i] * val

                return -1.0

        else:
            return -1.0
```

