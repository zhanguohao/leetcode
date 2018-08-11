# LeetCode: 417. 太平洋大西洋水流问题

[TOC]

## 1、题目描述

给定一个 `m x n` 的非负整数矩阵来表示一片大陆上各个单元格的高度。“太平洋”处于大陆的左边界和上边界，而“大西洋”处于大陆的右边界和下边界。

规定水流只能按照上、下、左、右四个方向流动，且只能从高到低或者在同等高度上流动。

请找出那些水流既可以流动到“太平洋”，又能流动到“大西洋”的陆地单元的坐标。

 

**提示：**

1. 输出坐标的顺序不重要
2. *m* 和 *n* 都小于150

**示例：**

```
给定下面的 5x5 矩阵:

  太平洋 ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * 大西洋

返回:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (上图中带括号的单元).
```

##  2、解题思路

​	这道题使用深度优先搜索即可，对于每一个坐标，判断是不是能够到太平洋和大西洋，如果都能到，假如结果集中即可

- 判断当前节点是不是在太平洋边界上，如果不是，深度优先所搜索，向上和想左，判断能否到达

- 大西洋也是如此

  如果对每一个节点判断能否到大西洋或者太平洋，有一点麻烦，计算量太大了，可以换个角度，找出所有能够流到大西洋的节点，找出所有能够流到太平洋的节点，然后将它们相交的点放入结果集中即可



```python
class Solution:
    def pacificAtlantic(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[List[int]]
        """
        
        
        row = len(matrix)
        if not row:
            return []
        col = len(matrix[0])
        a_visited = [[0] * col for _ in range(row)]
        b_visited = [[0] * col for _ in range(row)]

        directions = [(0, 1), (1, 0), (-1, 0), (0, -1)]

        ans = []

        def dfs(x, y, visited):
            visited[x][y] = 1
            for i, j in directions:
                m = x + i
                n = y + j
                if 0 <= m < row and 0 <= n < col and not visited[m][n] and matrix[m][n] >= matrix[x][y]:
                    dfs(m, n, visited)

        for r in range(row):
            dfs(r, 0, a_visited)
            dfs(r, col - 1, b_visited)

        for c in range(col):
            dfs(0, c, a_visited)
            dfs(row - 1, c, b_visited)

        for r in range(row):
            for c in range(col):
                if a_visited[r][c] and b_visited[r][c]:
                    ans.append([r, c])
        return ans
```



