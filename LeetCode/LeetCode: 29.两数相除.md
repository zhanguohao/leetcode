# LeetCode: 29.两数相除

[TOC]



## 1、题目描述

给定两个整数，被除数 `dividend` 和除数 `divisor`。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 `dividend` 除以除数 `divisor` 得到的商。

**示例 1:**

```
输入: dividend = 10, divisor = 3
输出: 3
```

**示例 2:**

```
输入: dividend = 7, divisor = -3
输出: -2
```

**说明:**

- 被除数和除数均为 32 位有符号整数。
- 除数不为 0。
- 假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−231,  231 − 1]。本题中，如果除法结果溢出，则返回 231 − 1。



## 2、解题思路

​	首先，如果当前数大于除数，我们就循环判断，为了加快进度，使用移位操作，如果大于其二倍，被除数左移一位，每一次左移一位，直到大于除数停止，这时候，我们减去这个值，然后继续判断，直到最后





```python
class Solution:
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        num1 = abs(dividend)
        num2 = abs(divisor)

        res = 0

        while num1 >= num2:
            temp = num2
            count = 1
            while (temp << 1) < num1:
                temp = temp << 1
                count = count << 1
            res += count
            num1 -= temp
        if (dividend > 0) ^ (divisor > 0):
            return -res
        if res >= 2 ** 31:
            return 2 ** 31-1
        return res
```

