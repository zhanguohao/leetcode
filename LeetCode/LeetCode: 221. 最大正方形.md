# LeetCode: 221. 最大正方形

[TOC]



## 1、题目描述



在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

**示例:**

```
输入: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```



## 2、解题思路

​	看到这道题，首先就想到的是使用动态规划来做

- 如果左上角的3个都不为0，继续判断
- 最小值正方形的边长加一
- 更新最大正方形



```python
class Solution:
    def maximalSquare(self, matrix):
        """
        :type matrix: List[List[str]]
        :rtype: int
        """
        row = len(matrix)
        if row <= 0:
            return 0
        col = len(matrix[0])
        buff = [[0] * col for i in range(row)]

        result = 0

        for i in range(col):
            if matrix[0][i] == '1':
                buff[0][i] = 1
                result = max(result, 1)
        for i in range(row):
            if matrix[i][0] == '1':
                buff[i][0] = 1
                result = max(result, 1)
        for i in range(1, row):
            for j in range(1, col):
                if matrix[i][j] == '1':
                    min_value = min(buff[i - 1][j - 1], buff[i][j - 1], buff[i - 1][j])
                    if min_value != 0:
                        buff[i][j] = (int(math.sqrt(min_value)) + 1) ** 2
                    else:
                        buff[i][j] = 1
                    result = max(result, buff[i][j])

        return result
```



