# LeetCode: 733. 图像渲染

[TOC]



## 1、题目描述



有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。

给你一个坐标 `(sr, sc)` 表示图像渲染开始的像素值（行 ，列）和一个新的颜色值 `newColor`，让你重新上色这幅图像。

为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。

最后返回经过上色渲染后的图像。

**示例 1:**

```
输入: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
输出: [[2,2,2],[2,2,0],[2,0,1]]
解析: 
在图像的正中间，(坐标(sr,sc)=(1,1)),
在路径上所有符合条件的像素点的颜色都被更改成2。
注意，右下角的像素没有更改为2，
因为它不是在上下左右四个方向上与初始点相连的像素点。
```

**注意:**

- `image` 和 `image[0]` 的长度在范围 `[1, 50]` 内。
- 给出的初始点将满足 `0 <= sr < image.length` 和 `0 <= sc < image[0].length`。
- `image[i][j]` 和 `newColor` 表示的颜色值在范围 `[0, 65535]`内。



## 2、解题思路

​	

```c
void flood(int **image, int imageRowSize, int imageColSize, int sr, int sc, int oldColor, int newColor, int **sign) {


    if (sign[sr][sc] == 0) {
        sign[sr][sc] = 1;

        if (oldColor == image[sr][sc]) {
            image[sr][sc] = newColor;
        } else {
            return;
        }
        if (sr - 1 >= 0) {
            flood(image, imageRowSize, imageColSize, sr - 1, sc, oldColor, newColor, sign);
        }
        if (sc - 1 >= 0) {
            flood(image, imageRowSize, imageColSize, sr, sc - 1, oldColor, newColor, sign);
        }
        if (sr + 1 < imageRowSize) {
            flood(image, imageRowSize, imageColSize, sr + 1, sc, oldColor, newColor, sign);
        }
        if (sc + 1 < imageColSize) {
            flood(image, imageRowSize, imageColSize, sr, sc + 1, oldColor, newColor, sign);
        }
    } else {
        return;
    }

}


int **floodFill(int **image, int imageRowSize, int imageColSize, int sr, int sc, int newColor, int **columnSizes,
                int *returnSize) {
    *columnSizes = (int *) malloc(sizeof(int) * imageRowSize);
    for (int i = 0; i < imageRowSize; i++) {
        (*columnSizes)[i] = imageColSize;
    }
    *returnSize = imageRowSize;

    int **result = (int **) malloc(sizeof(int *) * imageRowSize);
    int **sign = (int **) malloc(sizeof(int *) * imageRowSize);
//    int sign[imageRowSize][imageColSize];


    int *line;
    for (int i = 0; i < imageRowSize; i++) {
        line = (int *) malloc(sizeof(int) * imageColSize);
        result[i] = line;
        line = (int *) malloc(sizeof(int) * imageColSize);
        sign[i] = line;
        for (int j = 0; j < imageColSize; j++) {
            result[i][j] = image[i][j];
            sign[i][j] = 0;
        }
    }


    flood(result, imageRowSize, imageColSize, sr, sc, image[sr][sc], newColor, sign);
    return result;


}
```



