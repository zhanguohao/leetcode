# LeetCode: 79. 单词搜索

[TOC]



## 1、题目描述



给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**示例:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true.
给定 word = "SEE", 返回 true.
给定 word = "ABCB", 返回 false.
```



## 2、解题思路

​	使用深度优先搜索，对每一个开头字母能够匹配的字母，开始深度优先搜索

例如

​	

```
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```

```
给定 word = "ABCCED"
```



首先，判断第一个字符是不是匹配，如果匹配，就用从这个点出发，搜索一遍看看能否找到

​	如果找到，返回True

​	对每一个能匹配到的第一个字符，都这样搜索，

​	如果全部都找不到，返回False



```python
class Solution:
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        row = len(board)
        col = len(board[0])
        for i in range(row):
            for j in range(col):
                if word[0] == board[i][j]:
                    temp_visited = [[0] * col for _ in range(row)]
                    if self.findWord(board, word, 0, temp_visited, i, j, row, col):
                        return True
        return False

    def findWord(self, board, word, index, visited, x, y, row, col):
        if index >= len(word):
            return True
        if x < 0 or y < 0 or x >= row or y >= col:
            return False
        result = False

        # temp_visited = [[visited[i][j] for j in range(col)] for i in range(row)]
        if visited[x][y] == 0 and board[x][y] == word[index]:
            visited[x][y] = 1
            result = result or self.findWord(board, word, index + 1, visited, x - 1, y, row, col)
            result = result or self.findWord(board, word, index + 1, visited, x, y - 1, row, col)
            result = result or self.findWord(board, word, index + 1, visited, x + 1, y, row, col)
            result = result or self.findWord(board, word, index + 1, visited, x, y + 1, row, col)
        else:
            return False
        visited[x][y] = 0
        return result
```





