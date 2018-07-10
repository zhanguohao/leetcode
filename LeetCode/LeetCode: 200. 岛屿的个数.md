# LeetCode: 200. 岛屿的个数

[TOC]



## 1、题目描述



给定一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，计算岛屿的数量。一个岛被水包围，并且它是通过水平方向或垂直方向上相邻的陆地连接而成的。你可以假设网格的四个边均被水包围。

**示例 1:**

```
输入:
11110
11010
11000
00000

输出: 1
```

**示例 2:**

```
输入:
11000
11000
00100
00011

输出: 3
```



## 2、解题思路

​	这道题目比较简单，直接只用深度优先搜索就可以了

- 建立访问路径数组
- 遇到1，并且未访问过，就调用深度优先访问，将所有的相邻节点进行标记

```python
class Solution:
    def numIslands(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        row = len(grid)
        if row <= 0:
            return 0
        col = len(grid[0])

        visited = [[0] * col for i in range(row)]
        result = 0
        for i in range(row):
            for j in range(col):
                if grid[i][j] == '1' and visited[i][j] == 0:
                    self.dfs(i, j, row, col, grid, visited)
                    result += 1

        return result

    def dfs(self, i, j, row, col, grid, visited):
        if i >= 0 and i < row and j >= 0 and j < col and grid[i][j] == '1' and visited[i][j] == 0:
            visited[i][j] = 1
            self.dfs(i - 1, j, row, col, grid, visited)
            self.dfs(i, j - 1, row, col, grid, visited)
            self.dfs(i + 1, j, row, col, grid, visited)
            self.dfs(i, j + 1, row, col, grid, visited)
```

