# LeetCode: 840. 矩阵中的幻方

[TOC]



## 1、题目描述





3 x 3 的幻方是一个填充有**从 1 到 9** 的不同数字的 3 x 3 矩阵，其中每行，每列以及两条对角线上的各数之和都相等。

给定一个由整数组成的 N × N 矩阵，其中有多少个 3 × 3 的 “幻方” 子矩阵？（每个子矩阵都是连续的）。

 

**示例 1:**

```
输入: [[4,3,8,4],
      [9,5,1,9],
      [2,7,6,2]]
输出: 1
解释: 
下面的子矩阵是一个 3 x 3 的幻方：
438
951
276

而这一个不是：
384
519
762

总的来说，在本示例所给定的矩阵中只有一个 3 x 3 的幻方子矩阵。
```

**提示:**

1. `1 <= grid.length = grid[0].length <= 10`
2. `0 <= grid[i][j] <= 15`



## 2、解题思路

​	首先，不断地取出3*3的矩阵，用来判断是不是幻方

​	在判断的时候，首先，判断有没有重复数字，有的话，直接返回False

```python
class Solution:
    def numMagicSquaresInside(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        
        row = len(grid)
        col = len(grid[0])

        result = 0

        nine_num = []
        for i in range(row):
            if i + 2 < row:
                for j in range(col):
                    if j + 2 < col:
                        nine_num.clear()
                        nine_num.append(grid[i][j:j + 3])
                        nine_num.append(grid[i + 1][j:j + 3])
                        nine_num.append(grid[i + 2][j:j + 3])
                        if self.isMagic(nine_num):
                            result += 1
        return result

    def isMagic(self, grid):

        nums = set([1, 2, 3, 4, 5, 6, 7, 8, 9])
        nums_grid = set(grid[0] + grid[1] + grid[2])

        if len(nums_grid) < 9 or nums.difference(nums_grid) != set() :
            return False

        total = sum(grid[0])

        if sum(grid[1]) != total or sum(grid[2]) != total:
            return False
        if sum([v[0] for v in grid]) != total or sum([v[0] for v in grid]) != total or sum(
                [v[0] for v in grid]) != total:
            return False
        if sum([grid[0][0], grid[1][1], grid[2][2]]) != total or sum([grid[0][2], grid[1][1], grid[2][0]]) != total:
            return False

        return True
        
```

