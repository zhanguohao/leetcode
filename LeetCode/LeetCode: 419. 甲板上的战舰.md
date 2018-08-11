# LeetCode: 419. 甲板上的战舰

[TOC]

## 1、题目描述

给定一个二维的甲板， 请计算其中有多少艘战舰。 战舰用 `'X'`表示，空位用 `'.'`表示。 你需要遵守以下规则：

- 给你一个有效的甲板，仅由战舰或者空位组成。
- 战舰只能水平或者垂直放置。换句话说,战舰只能由 `1xN` (1 行, N 列)组成，或者 `Nx1` (N 行, 1 列)组成，其中N可以是任意大小。
- 两艘战舰之间至少有一个水平或垂直的空位分隔 - 即没有相邻的战舰。

**示例 :**

```
X..X
...X
...X
```

在上面的甲板中有2艘战舰。

**无效样例 :**

```
...X
XXXX
...X
```

你不会收到这样的无效甲板 - 因为战舰之间至少会有一个空位将它们分开。

**进阶:**

你可以用**一次扫描算法**，只使用**O(1)额外空间，**并且**不修改**甲板的值来解决这个问题吗？

## 2、解题思路

​	一开始没有理解题意，实际上，应该这样描述：

- 甲板上，'.'代表空位，'X'代表有战舰
- 战舰可能占有一个'X'或多个，一个战舰只能横着摆放或者竖着摆放
- 战舰之间必须留有空位（不然出不去了）

```
X..X.
.XXX.
...X.
```

如上，这就不对的，战舰放在了一起，没法区别是什么样的战舰了

```
X..X.
.XX..
...X.
```

这样就是正确的了



​	根据之前的描述，可以这样来判判定：

- 如果遇到'X'，判断这个点上方和左方有没有'X'，如果有，表示这个点是战舰的一部分，不增加计数
- 如果左面和上面都没有'X'，计数加一即可
- 遍历完成即可得

````python
class Solution:
    def countBattleships(self, board):
        """
        :type board: List[List[str]]
        :rtype: int
        """
        row = len(board)
        if not row:
            return 0
        col = len(board[0])
        ans = 0

        for i in range(row):
            for j in range(col):
                if board[i][j] == 'X':
                    if i == 0:
                        if j == 0:
                            ans += 1
                        elif board[i][j - 1] != 'X':
                            ans += 1
                    else:
                        if j == 0:
                            if board[i - 1][j] != 'X':
                                ans += 1
                        else:
                            if board[i - 1][j] != 'X' and board[i][j - 1] != 'X':
                                ans += 1
        return ans
````



​	判断条件还可以反过来写，这样判断起来更简单一些

```python
class Solution:
    def countBattleships(self, board):
        """
        :type board: List[List[str]]
        :rtype: int
        """
        
        row = len(board)
        if not row:
            return 0
        col = len(board[0])
        ans = 0

        for i in range(row):
            for j in range(col):
                if board[i][j] == '.' or (i > 0 and board[i - 1][j] == 'X') or (j > 0 and board[i][j - 1] == 'X'):
                    continue
                ans += 1
        return ans
```

​	不过这样效率反而慢一些。。