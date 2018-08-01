# LeetCode: 372. 超级次方

[TOC]



## 1、题目描述

你的任务是计算 *a**b* 对 1337 取模，*a* 是一个正整数，*b* 是一个非常大的正整数且会以数组形式给出。

**示例 1:**

```
a = 2
b = [3]

结果: 8
```

**示例 2:**

```
a = 2
b = [1,0]

结果: 1024
```

**致谢：**

特别感谢 [@Stomach_ache](https://leetcode.com/stomach_ache) 添加这道题并创建所有测试用例。



## 2、解题思路

​	这个是数学问题，一开始没什么思路，不过主要还是利用取模的公式，不断地减少数量级

```
 x * y mod p
 =
 (x mod p) * (y mod p) mod p
```

​	

```python
class Solution:
    def superPow(self, a, b):
        """
        :type a: int
        :type b: List[int]
        :rtype: int
        """
        p = int("".join(map(str, b)))
        a = a % 1337
        return self.calculate(a, p)

    def calculate(self, a, b):
        if b == 0:
            return 1
        if b == 1:
            return a % 1337

        val = self.calculate(a, b // 2)
        val *= val
        # 如果是奇数
        if b & 1 != 0:
            val *= a
        return val % 1337
```

