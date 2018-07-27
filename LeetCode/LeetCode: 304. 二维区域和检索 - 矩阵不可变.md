# LeetCode: 304. 二维区域和检索 - 矩阵不可变

[TOC]

## 1、题目描述

给定一个二维矩阵，计算其子矩形范围内元素的总和，该子矩阵的左上角为 (*row*1, *col*1) ，右下角为 (*row*2, *col*2)。

![Range Sum Query 2D](https://leetcode-cn.com/static/images/courses/range_sum_query_2d.png)
上图子矩阵左上角 (row1, col1) = **(2, 1)** ，右下角(row2, col2) = **(4, 3)，**该子矩形内元素的总和为 8。

**示例:**

```
给定 matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

**说明:**

1. 你可以假设矩阵不可变。
2. 会多次调用 *sumRegion* 方法*。*
3. 你可以假设 *row*1 ≤ *row*2 且 *col*1 ≤ *col*2。



## 2、解题思路

​	使用动态规划，每一个节点都是左上角节点到当前节点的和，

- `dp[i][j] = dp[i-1][j]+dp[i][j-1] - dp[i-1][j-1]`

上面是状态转换函数，由此，我们可以方便的求解出从左上角节点到当前节点



```python
class NumMatrix:

    def __init__(self, matrix):
        """
        :type matrix: List[List[int]]
        """
        
        
        self.matrix = matrix
        self.row = len(matrix)
        if self.row == 0:
            return
        self.col = len(matrix[0])
        if self.col == 0:
            return

        self.dp = [[0] * (self.col + 1) for _ in range(self.row + 1)]

        for i in range(self.row):
            for j in range(self.col):
                self.dp[i + 1][j + 1] = self.dp[i][j + 1] + self.dp[i + 1][j] - self.dp[i][j] + self.matrix[i][j]

        # print(self.matrix)
        # print(self.dp)

    def sumRegion(self, row1, col1, row2, col2):
        """
        :type row1: int
        :type col1: int
        :type row2: int
        :type col2: int
        :rtype: int
        """
        if self.row == 0 or self.col == 0:
            return 0
        return self.dp[row2 + 1][col2 + 1] - self.dp[row1][col2 + 1] - self.dp[row2 + 1][col1] + self.dp[row1][col1]

# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```





