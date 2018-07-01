# LeetCode: 400. 第N个数字

[TOC]



## 1、题目描述



在无限的整数序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...中找到第 *n* 个数字。

**注意:**
*n* 是正数且在32为整形范围内 ( *n* < 231)。

**示例 1:**

```
输入:
3

输出:
3
```

**示例 2:**

```
输入:
11

输出:
0

说明:
第11个数字在序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... 里是0，它是10的一部分。
```



## 2、解题思路





​	首先确认是几位数

​	然后根据是几位数中的第几个数，找到那个数，然后确认是这个数中的第几个数字



```c
int findNthDigit(int n) {
    
    
    int plus = 1;
    long digital = 9;
    int prev = 0;
    int pos = n;

    while (pos > plus * digital) {
        pos -= (int) plus * digital;
        plus++;
        digital *= 10;
    }

    prev = pos;

    pos = plus - (prev % plus == 0 ? plus : prev % plus);
    int num = (int) pow(10, plus - 1) + (prev-1) / plus;


    num = (num / (int) pow(10, pos)) % 10;

    return num;
    
}
```

