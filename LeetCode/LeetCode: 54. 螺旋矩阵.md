# LeetCode: 54. 螺旋矩阵

[TOC]



## 1、题目描述





给定一个包含 *m* x *n* 个元素的矩阵（*m* 行, *n* 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

**示例 1:**

```
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
```

**示例 2:**

```
输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
```





## 2、解题思路

​	看到这个题，首先，想到的就是像贪吃蛇一样，不断地向前走，然后遇到障碍以后，拐弯，不过贪吃蛇只能向前，向左，向右，不能回退

​	这样的话，我们就设计一个表示已经走过的路径的墙壁，表示已经走过的路

例如是2*2的矩阵，

```
1 2
3 4
我们要的顺序就是1 2 4 3

如何控制方向呢？
1 1 1 1
1 0 0 1
1 0 0 1
1 1 1 1

设计一个矩阵，专门用来表示走过的路径，最外围的是墙，然后每走过一步，就将该位置一
没一次走之前，都要判断下一位是0才可以，不然就要调转方向
如果无路可走，跳出即可

```





```python
import numpy as np

class Solution:
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        row = len(matrix)
        
        if row == 0:
            return []
        
        col = len(matrix[0])
        
        buff = np.zeros((row + 2, col + 2), dtype=int)

        for i in range(row + 2):
            buff[i][0] = 1
            buff[i][col + 1] = 1
        for i in range(col + 2):
            buff[0][i] = 1
            buff[row + 1][i] = 1

        result = []
        """
            方向：
                右 0
                下 1
                左 2
                上 3
        """
        direct = 0
        print(buff)

        x = 0
        y = -1

        finish = False

        while not finish:
            x_temp = x
            y_temp = y
            if direct == 0:
                y += 1
            elif direct == 1:
                x += 1
            elif direct == 2:
                y -= 1
            elif direct == 3:
                x -= 1

            if buff[x + 1][y + 1] == 0:
                result.append(matrix[x][y])
                buff[x + 1][y + 1] = 1
            else:
                x = x_temp
                y = y_temp
                direct = (direct + 1) % 4
                if buff[x_temp][y_temp + 1] == 1 and buff[x_temp + 1][y_temp] == 1 and buff[x_temp + 2][
                            y_temp + 1] == 1 and buff[x_temp + 1][y_temp + 2] == 1:
                    finish = True
        return result
```

