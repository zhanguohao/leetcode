# LeetCode: 64. 最小路径和

[TOC]



## 1、解题思路





给定一个包含非负整数的 *m* x *n* 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

**示例:**

```
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。
```





## 2、题目描述

​	这道题看起来与前面62，63题没什么区别，都是从右上角到右下角，不过这一个求解最少的路径花费，简单的做法就是贪心法，每一步都选最小花费的那个，最终得到的就是最小花费

​	

```python
class Solution:
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        row = len(grid)
        col = len(grid[0])

        for i in range(row):
            for j in range(col):
                if i - 1 >= 0 and j - 1 >= 0:
                    grid[i][j] = min(grid[i][j - 1], grid[i - 1][j]) + grid[i][j]
                elif i - 1 >= 0:
                    grid[i][j] = grid[i - 1][j] + grid[i][j]
                elif j - 1 >= 0:
                    grid[i][j] = grid[i][j - 1] + grid[i][j]

        return grid[row - 1][col - 1]
```

​	