# LeetCode: 240. 搜索二维矩阵 II

[TOC]



## 1、题目描述



编写一个高效的算法来搜索 *m* x *n* 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

**示例:**

现有矩阵 matrix 如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

给定 target = `5`，返回 `true`。

给定 target = `20`，返回 `false`。

## 2、解题思路

​	思路很简单，因为行是升序，列也是升序，

- 按照斜对角线，找出两个元素，这两个元素组成了一个小的矩阵

- 根据规则，左上角的是矩阵中最小的，右下角的是最大的

  如果是方阵，这样就出来了

  如果不是方阵，想要定位就要换个方法



同样是从左上角右下角开始寻找，找出两个点，

左上角的那个点要小于target

右下角的那个点要大于target



这两个点要组成与各矩形，并且是左面的在上，右面的在下的话，就满足条件，进行查找



然后，我们就排除了左上角和右下角的元素





```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

寻找20

```
从左上角开始，找到小于20的那个元素找到17

然后从右下角开始，先向左
找到23的时候，就已经不符合矩阵的形成了

向上，找到22，也不符合矩阵形成，退出

```



```python
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        
        row = len(matrix)
        if row <= 0:
            return False
        col = len(matrix[0])
        if col <= 0:
            return False

        diagonal = min(row, col)

        left_up = [0, 0]

        for i in range(diagonal):
            if matrix[left_up[0]][left_up[1]] == target:
                return True
            if left_up[0] < diagonal - 1 and matrix[left_up[0]][left_up[1]] < target:
                left_up[0] += 1
                left_up[1] += 1
            else:
                if left_up[0] > 0:
                    left_up[0] -= 1
                    left_up[1] -= 1
                break

        right_up = [row - 1, col - 1]
        for i in range(diagonal):
            if matrix[right_up[0]][right_up[1]] == target:
                return True
            if right_up[0] - diagonal > 0 and right_up[1] - diagonal > 0 and matrix[right_up[0]][right_up[1]] > target:
                right_up[0] -= 1
                right_up[1] -= 1

            else:
                if right_up[0] < row - 1:
                    right_up[0] += 1
                    right_up[1] += 1
                break

        if left_up[0] <= right_up[0] and left_up[1] <= right_up[1]:

            for i in range(left_up[0], right_up[0] + 1):
                for j in range(right_up[1] + 1):
                    if matrix[i][j] == target:
                        return True

            for i in range(left_up[0]):
                for j in range(left_up[1], right_up[1] + 1):
                    if matrix[i][j] == target:
                        return True

        return False

```

