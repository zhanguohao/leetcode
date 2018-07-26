# LeetCode: 289. 生命游戏

[TOC]



## 1、题目描述



根据[百度百科](https://baike.baidu.com/item/%E7%94%9F%E5%91%BD%E6%B8%B8%E6%88%8F/2926434?fr=aladdin)，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在1970年发明的细胞自动机。

给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞具有一个初始状态 *live*（1）即为活细胞， 或 *dead*（0）即为死细胞。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

1. 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
2. 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
3. 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
4. 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

根据当前状态，写一个函数来计算面板上细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

**示例:**

```
输入: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
输出: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

**进阶:**

- 你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。
- 本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？



## 2、解题思路

​	最简单莫过于直接创建一个一模一样的矩阵，更新状态以后，复制返回

​	题目要求原地算法，所以就要想办法，在原地就能够区分状态前与状态后的值

一共两种状态，0，1

状态的变化一共4种情况，0->0,  0->1, 1->0, 1->1

我们分别用2，3，4，5表示这四种状态，就能够得到更新前后的结果

最后，更新一遍状态，2，4更新为0，3，5更新为1即可

```python
class Solution:
    def gameOfLife(self, board):
        """
        :type board: List[List[int]]
        :rtype: void Do not return anything, modify board in-place instead.
        """
        
        row = len(board)
        if row <= 0:
            return

        col = len(board[0])
        if col <= 0:
            return

        """
            0->0: 2 
            0->1: 3
            1->0: 4
            1->1: 5
        """

        for i in range(row):
            for j in range(col):
                count = 0
                start_row = i - 1
                start_col = j - 1

                for k in range(9):
                    start_row = i - 1 + k // 3

                    start_col = j - 1 + k % 3

                    if 0 <= start_row < row and 0 <= start_col < col:
                        if board[start_row][start_col] in [1, 4, 5]:
                            count += 1

                if board[i][j] == 1:
                    count -= 1
                    if count < 2:
                        board[i][j] = 4
                    elif count <= 3:
                        board[i][j] = 5
                    else:
                        board[i][j] = 4
                else:
                    if count < 3:
                        board[i][j] = 2
                    elif count == 3:
                        board[i][j] = 3
                    else:
                        board[i][j] = 2

        for i in range(row):
            for j in range(col):
                if board[i][j] in [2, 4]:
                    board[i][j] = 0
                else:
                    board[i][j] = 1
```



