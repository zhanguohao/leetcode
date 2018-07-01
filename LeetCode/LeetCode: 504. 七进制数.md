# LeetCode: 504. 七进制数

[TOC]

## 1、题目描述



给定一个整数，将其转化为7进制，并以字符串形式输出。

**示例 1:**

```
输入: 100
输出: "202"
```

**示例 2:**

```
输入: -7
输出: "-10"
```

**注意:** 输入范围是 [-1e7, 1e7] 。





## 2、解题思路

​	直接用python写了，比较简单

```python
class Solution:
    def convertToBase7(self, num):
        """
        :type num: int
        :rtype: str
        """
        
        if num < 0:
            sign = "-"
        else:
            sign = ""
        temp = abs(num)
        result = str(temp % 7)

        while temp // 7:
            result = result + str(temp // 7 % 7)
            temp //= 7

        result += sign
        return "".join(reversed(result))
```

