# LeetCode: 566. 重塑矩阵

[TOC]



## 1、题目描述



在MATLAB中，有一个非常有用的函数 `reshape`，它可以将一个矩阵重塑为另一个大小不同的新矩阵，但保留其原始数据。

给出一个由二维数组表示的矩阵，以及两个正整数`r`和`c`，分别表示想要的重构的矩阵的行数和列数。

重构后的矩阵需要将原始矩阵的所有元素以相同的**行遍历顺序**填充。

如果具有给定参数的`reshape`操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

**示例 1:**

```
输入: 
nums = 
[[1,2],
 [3,4]]
r = 1, c = 4
输出: 
[[1,2,3,4]]
解释:
行遍历nums的结果是 [1,2,3,4]。新的矩阵是 1 * 4 矩阵, 用之前的元素值一行一行填充新矩阵。
```

**示例 2:**

```
输入: 
nums = 
[[1,2],
 [3,4]]
r = 2, c = 4
输出: 
[[1,2],
 [3,4]]
解释:
没有办法将 2 * 2 矩阵转化为 2 * 4 矩阵。 所以输出原矩阵。
```

**注意：**

1. 给定矩阵的宽和高范围在 [1, 100]。
2. 给定的 r 和 c 都是正数。



## 2、解题思路



​	实际思路很简单，创建了矩阵以后，从头开始赋值，

​	从第一行的第一个元素开始数

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** matrixReshape(int** nums, int numsRowSize, int numsColSize, int r, int c, int** columnSizes, int* returnSize) {
    // columnSizes = (int **) malloc(sizeof(int *));
    if (numsRowSize * numsColSize != r * c) {
        *returnSize = numsRowSize;
        *columnSizes = (int *) malloc(sizeof(int) * numsRowSize);
        for (int i = 0; i < numsRowSize; i++) {
            (*columnSizes)[i] = numsColSize;
        }
        return nums;
    }


    *returnSize = r;
    *columnSizes = (int *) malloc(sizeof(int) * r);
    for (int i = 0; i < r; i++) {
        (*columnSizes)[i] = c;
    }

    int **result = (int **) malloc(sizeof(int *) * r);
    int *line;
    for (int i = 0; i < r * c; i++) {
        if (i % c == 0) {
            line = (int *) malloc(sizeof(int) * c);
            result[i / c] = line;
        }
        result[i / c][i % c] = nums[i / numsColSize][i % numsColSize];
    }

    return result;
}
```

​	