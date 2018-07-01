# LeetCode: 461. 汉明距离

[TOC]



## 1、题目描述



两个整数之间的[汉明距离](https://baike.baidu.com/item/%E6%B1%89%E6%98%8E%E8%B7%9D%E7%A6%BB)指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 `x` 和 `y`，计算它们之间的汉明距离。

**注意：**
0 ≤ `x`, `y` < 231.

**示例:**

```
输入: x = 1, y = 4

输出: 2

解释:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

上面的箭头指出了对应二进制位不同的位置。
```



## 2、解题思路



```c
int hammingDistance(int x, int y) {
    int result = 0;
    for (int i = 0; i < 32; i++) {
        
        result += ((x & (1 << i)) ^ (y & (1 << i))) == 0 ? 0 : 1;
    }
    return result;
}
```





