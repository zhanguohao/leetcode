# LeetCode: 892. 三维形体的表面积

[TOC]

## 1、题目描述

在 `N * N` 的网格上，我们放置一些 `1 * 1 * 1 ` 的立方体。

每个值 `v = grid[i][j]` 表示 `v` 个正方体叠放在单元格 `(i, j)` 上。

返回结果形体的总表面积

**示例 1：**

```
输入：[[2]]
输出：10
```

**示例 2：**

```
输入：[[1,2],[3,4]]
输出：34
```

**示例 3：**

```
输入：[[1,0],[0,2]]
输出：16
```

**示例 4：**

```
输入：[[1,1,1],[1,0,1],[1,1,1]]
输出：32
```

**示例 5：**

```
输入：[[2,2,2],[2,1,2],[2,2,2]]
输出：46
```

 

**提示：**

- `1 <= N <= 50`
- `0 <= grid[i][j] <= 50`



## 2、解题思路

	这道题和前面887题不一样，那道题是投影，而这道题是表面积，区别就是，如果中间有些空的，或者说是一些凹的点，表面积会增加的

	基本的解题思路就是，我们针对每一个不为0的点，求解它对于总的表面积的增益

```
举个例子：
	当前有3个方块，然后在他的上下左右分别有1，2，3，4个方块，于是，它对于总的表面积的增益为：
	
	顶 底：	2
	 上  ：	2
	 下  ：   1
	 左  ：   0
	 右  ：   0
	 
	于是，这个点的面积增益就为5，加到总面积即可
```

```python
class Solution:
    def surfaceArea(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        
        row = len(grid)
        if not row:
            return 0
        col = len(grid[0])
        if not col:
            return 0

        border = [(1, 0), (0, 1), (-1, 0), (0, -1)]

        area_top_bottom = sum([1 if i else 0 for pos in grid for i in pos])

        ans = 2 * area_top_bottom

        for i in range(row):
            for j in range(col):
                if grid[i][j]:
                    for x, y in border:
                        ans += self.get_point(i, j, i + x, j + y, grid)

        return ans

    def get_point(self, x, y, border_x, border_y, grid):
        if border_x < 0 or border_x >= len(grid) or border_y < 0 or border_y >= len(grid[0]):
            return grid[x][y]
        else:
            if grid[border_x][border_y] >= grid[x][y]:
                return 0
            else:
                return grid[x][y] - grid[border_x][border_y]
```

