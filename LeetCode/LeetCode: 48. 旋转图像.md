# LeetCode: 48. 旋转图像

[TOC]



## 1、题目描述



给定一个 *n* × *n* 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

**说明：**

你必须在**原地**旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要**使用另一个矩阵来旋转图像。

**示例 1:**

```
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**示例 2:**

```
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```





## 2、解题思路

​	从最简单的进行分析，2*2的矩阵

假如是

```
1 2
3 4

反转90度，变成样子就是：
3 1
4 2

其实这个过程可以分成两步来做，
第一步，对角矩阵交换，也就是右上方的和左下方的进行交换
变成
1 3
2 4

然后每一行左右交换
3 1 
4 2

看到这里，实际上，我们就看到了没如果想要左转90度呢？
只要把第一步变成，左上角和右下角更改就好了
左转90度的结果是
2 4
1 3

那么第一步，左上角和右下角交换
4 2
3 1
第二步，每一行，左右交换


```



```python
class Solution:
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: void Do not return anything, modify matrix in-place instead.
        """
        row = len(matrix)
        col = len(matrix[0])

        for i in range(row):
            for j in range(i,col):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

        for i in range(row):
            for j in range(col // 2):
                matrix[i][j], matrix[i][col - 1 - j] = matrix[i][col - 1 - j], matrix[i][j]
```



