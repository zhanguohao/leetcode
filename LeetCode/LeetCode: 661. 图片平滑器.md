# LeetCode: 661. 图片平滑器

[TOC]



## 1、题目描述



包含整数的二维矩阵 M 表示一个图片的灰度。你需要设计一个平滑器来让每一个单元的灰度成为平均灰度 (向下舍入) ，平均灰度的计算是周围的8个单元和它本身的值求平均，如果周围的单元格不足八个，则尽可能多的利用它们。

**示例 1:**

```
输入:
[[1,1,1],
 [1,0,1],
 [1,1,1]]
输出:
[[0, 0, 0],
 [0, 0, 0],
 [0, 0, 0]]
解释:
对于点 (0,0), (0,2), (2,0), (2,2): 平均(3/4) = 平均(0.75) = 0
对于点 (0,1), (1,0), (1,2), (2,1): 平均(5/6) = 平均(0.83333333) = 0
对于点 (1,1): 平均(8/9) = 平均(0.88888889) = 0
```

**注意:**

1. 给定矩阵中的整数范围为 [0, 255]。
2. 矩阵的长和宽的范围均为 [1, 150]。



## 2、解题思路

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** imageSmoother(int** M, int MRowSize, int MColSize, int** columnSizes, int* returnSize) {
    *columnSizes = (int *) malloc(sizeof(int) * MRowSize);
    *returnSize = MRowSize;


    for (int i = 0; i < MRowSize; i++) {
        (*columnSizes)[i] = MColSize;
    }


    int sum = 0;
    int count = 0;
    int **result = (int **) malloc(sizeof(int *) * MRowSize);
    int *cur_line;

    int row;
    int col;
    for (int i = 0; i < MRowSize; i++) {
        cur_line = (int *) malloc(sizeof(int) * MColSize);
        result[i] = cur_line;
        for (int j = 0; j < MColSize; j++) {
            sum = 0;
            count = 0;
            for (int k = 0; k < 9; k++) {

                row = i - 1 + k / 3;
                col = j - 1 + k % 3;
                if (row >= 0 && row < MRowSize && col >= 0 && col < MColSize) {
                    sum += M[row][col];
                    count++;
                }
            }

            result[i][j] = sum / count;
        }
    }

    return result;
}
```









