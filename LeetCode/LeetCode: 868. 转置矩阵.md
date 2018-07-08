# LeetCode: 868. 转置矩阵

[TOC]



## 1、题目描述



给定一个矩阵 `A`， 返回 `A` 的转置矩阵。

矩阵的转置是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

 

**示例 1：**

```
输入：[[1,2,3],[4,5,6],[7,8,9]]
输出：[[1,4,7],[2,5,8],[3,6,9]]
```

**示例 2：**

```
输入：[[1,2,3],[4,5,6]]
输出：[[1,4],[2,5],[3,6]]
```



## 2、解题思路

​	这个题目很简单，直接构造目标数组，然后对应的赋值即可，只不过横坐标和纵坐标是相反的



```python
class Solution(object):
    def transpose(self, A):
        """
        :type A: List[List[int]]
        :rtype: List[List[int]]
        """
        row = len(A)
        if row <= 0:
            return A
        col = len(A[0])

        result = [[0] * row for i in range(col)]

        for i in range(col):
            for j in range(row):
                result[i][j] = A[j][i]

        return result
```

