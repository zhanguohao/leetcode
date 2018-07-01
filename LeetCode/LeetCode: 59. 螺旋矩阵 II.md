# LeetCode: 59. 螺旋矩阵 II

[TOC]

## 1、题目描述





给定一个正整数 *n*，生成一个包含 1 到 *n*2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

**示例:**

```
输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```





## 2、解题思路

​	这道题与前面的54题，螺旋矩阵相似，实际上，可以用相同的思路实现



```python
import numpy as np
class Solution:
    def generateMatrix(self, n):
        """
        :type n: int
        :rtype: List[List[int]]
        """
        row = n

        if row == 0:
            return []

        col = n

        buff = np.zeros((row + 2, col + 2), dtype=int)

        for i in range(row + 2):
            buff[i][0] = 1
            buff[i][col + 1] = 1
        for i in range(col + 2):
            buff[0][i] = 1
            buff[row + 1][i] = 1

            

        result = [ [0 for i in range(n)]  for i in range(n)]
        """
            方向：
                右 0
                下 1
                左 2
                上 3
        """
        direct = 0

        x = 0
        y = -1

        finish = False
        index = 1
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
                result[x][y] = index
                buff[x + 1][y + 1] = 1
                index += 1
            else:
                x = x_temp
                y = y_temp
                direct = (direct + 1) % 4
                if buff[x_temp][y_temp + 1] == 1 and buff[x_temp + 1][y_temp] == 1 and buff[x_temp + 2][
                            y_temp + 1] == 1 and buff[x_temp + 1][y_temp + 2] == 1:
                    finish = True
        return result
```



​	虽然通过了，不过耗费时间有点长，换个思路，再来一次



​	我们实际上只需要控制转向就可以了，然后判断现在是需要赋值哪些

​	首先，我们判断该如何赋值，找出规律

3*3的矩阵

首先是 第一行，坐标为

```
0,0
0,1
0,2
```



然后是最右面

```
1,2
2,2
```

然后是下面

```
2,1
2,0
```

再来左面

```
1，0
```

接下来又开始

```
1,1
```



其中有什么规律呢？

​	如果是从左向右走，只会增加纵坐标

​	从上往下走，只会增加横坐标

​	从右往左走，只会减少横坐标

​	从下向上，只会增加纵坐标

于是，我们就可以控制这个了，根据转向，该增加那个坐标，

设置4个变量，控制增加到什么位置结束



```python
import numpy as np
class Solution:
    def generateMatrix(self, n):
        """
        :type n: int
        :rtype: List[List[int]]
        """
        result = result = [[0] * n for _ in range(n)]
        """
            方向：
                右 0
                下 1
                左 2
                上 3
        """
        direct = 0

        finish = False
        index = 1

        row = 0
        col = 0

        # 这里直接设置边界，up表示横坐标值，是上面向右寻找，应该是从0到
        #

        right = n - 1
        down = n - 1
        left = 0
        up = 1

        while index <= n*n:
            if direct == 0:
                for i in range(col, right + 1):
                    result[row][i] = index
                    index += 1
                row += 1
                col = right
                right -= 1
                direct += 1
            elif direct == 1:
                for i in range(row, down + 1):
                    result[i][col] = index
                    index += 1
                row = down
                col -= 1
                down -= 1
                direct += 1
            elif direct == 2:
                for i in range(col, left - 1, -1):
                    result[row][i] = index
                    index += 1
                row -= 1
                col = left
                left += 1
                direct += 1

            elif direct == 3:
                for i in range(row, up - 1, -1):
                    result[i][col] = index
                    index += 1
                row = up
                col += 1
                up += 1
                direct += 1

            direct %= 4

        return result
```

​	没想到直接赋值，执行是时间也不快，算了，以后在优化，逻辑清晰更重要

