# LeetCode: 130. 被围绕的区域

[TOC]



## 1、题目描述



给定一个二维的矩阵，包含 `'X'` 和 `'O'`（**字母 O**）。

找到所有被 `'X'` 围绕的区域，并将这些区域里所有的 `'O'` 用 `'X'` 填充。

**示例:**

```
X X X X
X O O X
X X O X
X O X X
```

运行你的函数后，矩阵变为：

```
X X X X
X X X X
X X X X
X O X X
```

**解释:**

被围绕的区间不会存在于边界上，换句话说，任何边界上的 `'O'` 都不会被填充为 `'X'`。 任何不在边界上，或不与边界上的 `'O'` 相连的 `'O'` 最终都会被填充为 `'X'`。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。



## 2、解题思路

​	看到这道题，首先想到的就是利用dfs来搜索

​	基本思路就是乳沟在边界发现了一个'0'，就利用深度优先搜索，将所有的与之相邻的0并且不是边界的0标记位A

​	然后将所有的0变成X，

​	然后将左右的A变成0即可

​	注意，是字母$O$，不是数字$0$

```python
class Solution:
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: void Do not return anything, modify board in-place instead.
        """
        
        row = len(board)
        if row <= 2:
            return
        col = len(board[0])

        for i in range(row):
            if board[i][0] == 'O':
                self.dfs(board, i, 0)
            if board[i][col - 1] == 'O':
                self.dfs(board, i, col - 1)

        for i in range(col):
            if board[0][i] == 'O':
                self.dfs(board, 0, i)
            if board[row - 1][i] == 'O':
                self.dfs(board, row - 1, i)

        for i in range(row):
            for j in range(col):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                elif board[i][j] == 'A':
                    board[i][j] = 'O'

    def dfs(self, board, i, j):
        if i >= 0 and i < len(board) and j >= 0 and j < len(board[0]) and board[i][j] == "O":
            board[i][j] = 'A'
            self.dfs(board, i - 1, j)
            self.dfs(board, i, j - 1)
            self.dfs(board, i + 1, j)
            self.dfs(board, i, j + 1)
```

