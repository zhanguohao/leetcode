# LeetCode: 867. 回文素数

[TOC]

## 1、题目描述



求出大于或等于 `N` 的最小回文素数。

回顾一下，如果一个数大于 1，且其因数只有 1 和它自身，那么这个数是*素数*。

例如，2，3，5，7，11 以及 13 是素数。

回顾一下，如果一个数从左往右读与从右往左读是一样的，那么这个数是*回文数。*

例如，12321 是回文数。

 

**示例 1：**

```
输入：6
输出：7
```

**示例 2：**

```
输入：8
输出：11
```

**示例 3：**

```
输入：13
输出：101
```

 

**提示：**

- `1 <= N <= 10^8`



## 2、解题思路

​	这道题的基本思路就是，构造不小于N的回文数，判断是不是质数，如果是，就返回

​	关键点在于构造回文数

​	这里使用的是回文长度的思路

​	例如现在构造3位的回文数，基准值是10，第一个回文就是101

​	然后是111

​	如果构造4位的回文，基准值是10，第一个回文是1001，第二个是1111，以此类推



```
import math
class Solution:
    def primePalindrome(self, N):
        """
        :type N: int
        :rtype: int
        """
        
        p = str(N)

        p_length = len(p)
        base_num = 10 ** ((p_length - 1) // 2)
        if base_num == 1:
            base_num = 0

        while True:

            judge_num = self.buildPalindrome(base_num, p_length)

            if judge_num >= N:
                if self.isPrime(judge_num):
                    return judge_num

            if len(str(base_num + 1)) > len(str(base_num)):
                p_length += 1
                base_num = 10 ** ((p_length - 1) // 2)
            else:
                base_num += 1

    def buildPalindrome(self, num, p_length):

        s = str(num)

        if p_length % 2 == 0:
            result = int(s + "".join(list(reversed(s))))
        else:
            result = int(s[:-1] + s[-1] + "".join(list(reversed(s[:-1]))))
        return result

    def isPrime(self, num):
        if num <= 1:
            return False
        if num == 2:
            return True
        a = math.ceil(math.sqrt(num))
        for i in range(2, a + 1):
            if num % i == 0:
                return False
        return True
```

