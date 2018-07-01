# LeetCode: 69.x 的平方根

[TOC]

## 1、题目描述



实现 `int sqrt(int x)` 函数。

计算并返回 *x* 的平方根，其中 *x* 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

**示例 1:**

```
输入: 4
输出: 2
```

**示例 2:**

```
输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```





## 2、解题思路

​	使用二分查找法

​	需要注意int的上限值

​        $2^{31}-1 = 2147483647$

​	其最大值为：46341	







```c
int mySqrt(int x) {
    int max_int_sqrt = 46341;
    int left = 0;
    int right = x > max_int_sqrt ? max_int_sqrt : x;
    int left_temp = left;
    int right_temp = right;
    int temp;
    while (left < right - 1 && right <= 46341) {
        temp = right - (right - left) / 2;
        if (temp * temp > x) {
            right_temp = temp;
        } else {
            left_temp = temp;
        }
        temp = left + (right - left) / 2;
        if (temp * temp > x) {

            right = temp < right_temp ? temp : right_temp;

        } else {
            left = temp > left_temp ? temp : left_temp;
        }
    }


    if (right * right == x) {
        return right;
    } else {
        return left;
    }
}
```







​	将除以2改成右移一位

```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <memory.h>
#include <string.h>

/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int mySqrt(int x) {
    int max_int_sqrt = 46341;
    int left = 0;
    int right = x > max_int_sqrt ? max_int_sqrt : x;
    int left_temp = left;
    int right_temp = right;
    int temp;
    while (left < right - 1 && right <= 46341) {
        temp = right - ((right - left) >> 1);
        if (temp * temp > x) {
            right_temp = temp;
        } else {
            left_temp = temp;
        }
        temp = left + ((right - left) >> 1);
        if (temp * temp > x) {

            right = temp < right_temp ? temp : right_temp;

        } else {
            left = temp > left_temp ? temp : left_temp;
        }
    }


    if (right * right == x) {
        return right;
    } else {
        return left;
    }
}

int main() {
    for (int i = 1; i < 500; i++) {

        printf("number: %d\t sqrt: %d\n", i, mySqrt(i));
    }
    printf("number: %d\t sqrt: %d\n", 2147395599, mySqrt(2147395599));
    printf("number: %d\t sqrt: %d\n", 2147395600, mySqrt(2147395600));


}

```

