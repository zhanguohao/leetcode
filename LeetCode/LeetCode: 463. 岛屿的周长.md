# LeetCode: 463. 岛屿的周长

[TOC]



## 1、题目描述



给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

**示例 :**

```
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

答案: 16
解释: 它的周长是下面图片中的 16 个黄色的边：

```



![](https://leetcode-cn.com/static/images/problemset/island.png)





## 2、解题思路

​	解法很简单，遍历这个块，看看这个块周边是1，还是0，统计1的数量，然后用边数减去1的数量即可

```c
int islandPerimeter(int** grid, int gridRowSize, int gridColSize) {
    
    int result = 0;
    for (int i = 0; i < gridRowSize; i++) {
        for (int j = 0; j < gridColSize; j++) {
            if (grid[i][j]) {
                result += 4;
                if (i - 1 >= 0) {
                    if (grid[i - 1][j] == 1) {
                        result -= 1;
                    }
                }
                if (j - 1 >= 0) {
                    if (grid[i][j - 1] == 1) {
                        result -= 1;
                    }
                }
                if (i + 1 < gridRowSize) {
                    if (grid[i + 1][j] == 1) {
                        result -= 1;
                    }
                }
                if (j + 1 < gridColSize) {
                    if (grid[i][j + 1] == 1) {
                        result -= 1;
                    }
                }
            }
        }
    }


    return result;
}
```



