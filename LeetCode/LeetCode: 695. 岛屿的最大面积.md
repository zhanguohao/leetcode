# LeetCode: 695. 岛屿的最大面积

[TOC]

## 1、题目描述



给定一个包含了一些 0 和 1的非空二维数组 `grid` , 一个 **岛屿** 是由四个方向 (水平或垂直) 的 `1` (代表土地) 构成的组合。你可以假设二维矩阵的四个边缘都被水包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为0。)

**示例 1:**

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

对于上面这个给定矩阵应返回 `6`。注意答案不应该是11，因为岛屿只能包含水平或垂直的四个方向的‘1’。

**示例 2:**

```
[[0,0,0,0,0,0,0,0]]
```

对于上面这个给定的矩阵, 返回 `0`。

**注意:** 给定的矩阵`grid` 的长度和宽度都不超过 50。





## 2、解题思路

​	对每一个为1的点，发起深度优先搜索，向右和向下搜索，其实与二叉树是一样的，但是不同的是，直接搜索可能放问同样的节点，因此，使用一个访问标记即可，如果该节点访问过了，解放上标记1，没访问过，就是标记0

​	

```c
void dfs(int **grid, int **sign, int *max, int gridRowSize, int gridColSize, int i, int j) {
    if (i < 0 || i >= gridRowSize || j < 0 || j >= gridColSize) {
        return;
    }

    if (sign[i][j] == 0) {
        if (grid[i][j] == 1) {
            *max += 1;
            sign[i][j] = 1;
            dfs(grid, sign, max, gridRowSize, gridColSize, i - 1, j);
            dfs(grid, sign, max, gridRowSize, gridColSize, i, j - 1);
            dfs(grid, sign, max, gridRowSize, gridColSize, i + 1, j);
            dfs(grid, sign, max, gridRowSize, gridColSize, i, j + 1);
        }
    } else {
        return;
    }

}


int maxAreaOfIsland(int **grid, int gridRowSize, int gridColSize) {
    int result = 0;
    int max = 0;
    int **sign = (int **) malloc(sizeof(int *) * gridRowSize);
    for (int i = 0;i<gridRowSize;i++){
        sign[i] = (int *)malloc(sizeof(int)*gridColSize);
    }
    for (int i = 0; i < gridRowSize; i++) {
        for (int j = 0; j < gridColSize; j++) {
            sign[i][j] = 0;
        }
    }

    for (int i = 0; i < gridRowSize; i++) {
        for (int j = 0; j < gridColSize; j++) {
            if (grid[i][j] == 1) {
                max = 0;
                dfs(grid, sign, &max, gridRowSize, gridColSize, i, j);
                if (result < max) {
                    result = max;
                }
            }
        }
    }


    return result;


}
```

