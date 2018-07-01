# LeetCode: 66.加一

[TOC]

## 1、题目描述

给定一个**非负整数**组成的**非空**数组，在该数的基础上加一，返回一个新的数组。

最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

**示例 1:**

```
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。
```

**示例 2:**

```
输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。
```



## 2、解题思路

​	首先判断数组是不是需要扩展一位

​	然后申请空间

​	从尾部开始计算，遇到9，并且进位为1，则赋值0，并且进位仍为1

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* plusOne(int* digits, int digitsSize, int* returnSize) {
    int length = digitsSize;
    int extend = 1;
    int carry = 1;
    for (int i = 0; i < digitsSize; i++) {
        if (digits[i] != 9) {
            extend = 0;
            break;
        }
    }
    if (extend == 1) {
        length++;
    }

    int *result = (int *) malloc(sizeof(int) * length);

    for (int i = length - 1; (i-extend) >= 0; i--) {
        if (digits[i - extend] == 9 && carry == 1) {
            result[i] = 0;
        } else {
            result[i] = digits[i - extend] + carry;
            carry = 0;

        }
    }
    if (extend == 1) {
        result[0] = 1;
    }
    *returnSize = length;
    return result;
}
```

