# LeetCode: 263. 丑数

[TOC]

## 1、题目描述



编写一个程序判断给定的数是否为丑数。

丑数就是只包含质因数 `2, 3, 5` 的**正整数**。

**示例 1:**

```
输入: 6
输出: true
解释: 6 = 2 × 3
```

**示例 2:**

```
输入: 8
输出: true
解释: 8 = 2 × 2 × 2
```

**示例 3:**

```
输入: 14
输出: false 
解释: 14 不是丑数，因为它包含了另外一个质因数 7。
```

**说明：**

1. `1` 是丑数。
2. 输入不会超过 32 位有符号整数的范围: [−231,  231 − 1]。



## 2、解题思路

​	判断是不是不能被2，3，5整除，不能的话，返回false

​	可以的话，除以2，3，5，继续判断



```c
bool isUgly(int num) {
    if (num <= 0) {
        return false;
    }
    int result = num;
    int flag = 0;
    while (result != 1) {
        flag = 0;
        if (result % 2 != 0) {
            flag++;
        } else {
            result /= 2;
        }
        if (result % 3 != 0) {
            flag++;
        } else {
            result /= 3;
        }

        if (result % 5 != 0) {
            flag++;
        } else {
            result /= 5;
        }
        if (flag >= 3) {
            return false;
        }
    }

    return true;
}
```

