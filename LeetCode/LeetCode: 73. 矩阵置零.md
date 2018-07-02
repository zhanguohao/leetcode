# LeetCode: 73. 矩阵置零

[TOC]



## 1、题目描述



给定一个 *m* x *n* 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用**原地**算法**。**

**示例 1:**

```
输入: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
输出: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

**示例 2:**

```
输入: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
输出: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

**进阶:**

- 一个直接的解决方案是使用  O(*m**n*) 的额外空间，但这并不是一个好的解决方案。
- 一个简单的改进方案是使用 O(*m* + *n*) 的额外空间，但这仍然不是最好的解决方案。
- 你能想出一个常数空间的解决方案吗？



## 2、解题思路

​	实际上，如上面进阶所说的 ，使用额外空间保存的话，题目很简单，但是，如果不用额外空间呢？

​	只需要在数组中，使用第0行和第0列来打标记就可以了，然后通过编列第0行和第0列，将对应的行与列置零

​	

​	这里需要注意的就是，左上角的元素需要存储，单独设置一个变量存储即可，

​	如果是行元素或者列元素将它置1，就设置为1，如果本身就是0，设置为2

```python
class Solution:
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: void Do not return anything, modify matrix in-place instead.
        """
        
        row = len(matrix)
        col = len(matrix[0])

        # top 存储左上角的交叉点的状态
        # 1: 行置0
        # 2: 列置0
        # 3: 行列都置0
        top = 0

        for i in range(row):
            for j in range(col):

                if matrix[i][j] == 0:

                    if i == 0 and j == 0:
                        top = 3

                    elif top != 3:
                        if i == 0:
                            if top == 2:
                                top = 3
                            else:
                                top = 1
                        if j == 0:
                            if top == 1:
                                top = 3
                            else:
                                top = 2

                        if top == 2 and i == 0:
                            top = 3
                        elif top == 1 and j == 0:
                            top = 3

                    matrix[i][0] = 0
                    matrix[0][j] = 0

        for i in range(1, row):
            if matrix[i][0] == 0:
                for j in range(col):
                    matrix[i][j] = 0

        for i in range(1, col):
            if matrix[0][i] == 0:
                for j in range(row):
                    matrix[j][i] = 0

        if top == 3:
            for i in range(row):
                matrix[i][0] = 0
            for i in range(col):
                matrix[0][i] = 0
        elif top == 1:
            for i in range(col):
                matrix[0][i] = 0
        elif top == 2:
            for i in range(row):
                matrix[i][0] = 0
```









