# LeetCode: 264. 丑数 II

[TOC]

## 1、题目描述

编写一个程序，找出第 `n` 个丑数。

丑数就是只包含质因数 `2, 3, 5` 的**正整数**。

**示例:**

```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

**说明:**  

1. `1` 是丑数。
2. `n` **不超过**1690。

## 2、解题思路

​	一个丑数，肯定是前面的丑数中乘以2，3，5以后，最小的那个

​	根据这个思路，我们维护3个数，使用第几个数字乘以2，3，5

​	每一次取其中的最小值

​	然后用过了以后，下标向后移动

​	

```
[1]
0 0 0
```

```
[1 2]
1 0 0
```

```
[1 2 3]
1 1 0
```

```
[1 2 3 4]
2 1 0
```

```python
class Solution:
    def nthUglyNumber(self, n):
        """
        :type n: int
        :rtype: int
        """
        buff = [1]

        p2 = 0
        p3 = 0
        p5 = 0

        for i in range(n - 1):

            temp2 = buff[p2] * 2
            temp3 = buff[p3] * 3
            temp5 = buff[p5] * 5
            buff.append(min(temp2, temp3, temp5))
            if buff[-1] == temp2:
                p2 += 1
            if buff[-1] == temp3:
                p3 += 1
            if buff[-1] == temp5:
                p5 += 1

        return buff[-1]
```

