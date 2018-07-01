# LeetCode: 118. 杨辉三角

[TOC]

## 1、题目描述

给定一个非负整数 *numRows，*生成杨辉三角的前 *numRows* 行。

![img](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

在杨辉三角中，每个数是它左上方和右上方的数的和。

**示例:**

```
输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

## 2、解题思路

```c
/**
 * Return an array of arrays.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** generate(int numRows, int** columnSizes) {
     if (numRows <= 0) {
        return NULL;
    }
    int cur_pos = 0;
    int **result = (int **) malloc(sizeof(int *) * numRows);
    int *cur_line;
    *columnSizes = (int *) malloc(sizeof(int) * numRows);
    for (int count = 1; count <= numRows; count++) {
        (*columnSizes)[count - 1] = count;
        cur_line = (int *) malloc(sizeof(int) * count);
        result[count - 1] = cur_line;
        cur_line[cur_pos++] = 1;
        while (cur_pos  < (count - 1)) {
            cur_line[cur_pos] = result[count - 2][cur_pos - 1] + result[count - 2][cur_pos];
            cur_pos++;
        }
        if(count>1){
            cur_line[cur_pos] = 1;
        }

        cur_pos = 0;

    }

    return result;
}
```

