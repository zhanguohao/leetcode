# LeetCode: 306. 累加数

[TOC]



## 1、题目描述



累加数是一个字符串，组成它的数字可以形成累加序列。

一个有效的累加序列必须**至少**包含 3 个数。除了最开始的两个数以外，字符串中的其他数都等于它之前两个数相加的和。

给定一个只包含数字 `'0'-'9'` 的字符串，编写一个算法来判断给定输入是否是累加数。

**说明:** 累加序列里的数不会以 0 开头，所以不会出现 `1, 2, 03` 或者 `1, 02, 3` 的情况。

**示例 1:**

```
输入: "112358"
输出: true 
解释: 累加序列为: 1, 1, 2, 3, 5, 8 。1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```

**示例 2:**

```
输入: "199100199"
输出: true 
解释: 累加序列为: 1, 99, 100, 199。1 + 99 = 100, 99 + 100 = 199
```

**进阶:**
你如何处理一个溢出的过大的整数输入?



## 2、解题思路

​	输入的是字符串，那么就不能使用常规的计算，不能够先将字符串变成数字，然后相加，需要使用字符串加法来做

​	当然，如果是很短的字符串，也没必要，不过，使用字符串加法，可以防止溢出问题

​	然后关键就在于确定前两个数，只要确定了前两个数，整个序列就确定下来了

​	

```python
class Solution:
    def isAdditiveNumber(self, num):
        """
        :type num: str
        :rtype: bool
        """
        
        length = len(num)
        if length <= 2:
            return False

        for i in range(1, length // 2 + 1):
            s1 = num[:i]

            j = 1
            while (length - i - j) >= max(i, j):
                s2 = num[i:i + j]

                if len(s2) > 1 and s2[0] == '0':
                    j += 1
                    continue
                if self.judgePlusString(s1, s2, num):
                    return True
                j += 1

        return False

    def judgePlusString(self, s1, s2, s3):

        length1 = len(s1)
        length2 = len(s2)

        if (length1 > 1 and s1[0] == '0') or (length2 > 1 and s2[0] == '0'):
            return False

        res = ""
        carry = 0

        pre_part = min(length1, length2)

        for i in range(1, max(length1, length2) + 1):
            if i <= length1 and i <= length2:
                digit = ord(s1[-i]) + ord(s2[-i]) - 96 + carry
            elif i <= length1:
                digit = ord(s1[-i]) - 48 + carry
            else:
                digit = ord(s2[-i]) - 48 + carry
            carry = digit // 10
            res = str(digit % 10) + res

        if carry:
            res = str(carry) + res

        temp = s1 + s2 + res

        if temp == s3:
            return True
        elif temp == s3[:len(temp)]:
            return self.judgePlusString(s2, res, s3[len(s1):])
        else:
            return False
```

