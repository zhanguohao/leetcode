# LeetCode: 357. 计算各个位数不同的数字个数

[TOC]



## 1、题目描述



给定一个非负整数 n，计算各位数字都不同的数字 x 的个数，其中 0 ≤ x < 10n。

**示例:**
给定 n = 2，返回 91。（答案应该是除`[11,22,33,44,55,66,77,88,99]`外，0 ≤ x < 100 间的所有数字）

**致谢:**
特别感谢 [@memoryless](https://discuss.leetcode.com/user/memoryless) 添加这个题目并创建所有测试用例。

 

## 2、解题思路

​	分几种情况讨论

- 如果n为0，只有一个数，0，返回结果1
- 如果n为1，有0-9，10个数，返回10
- 如果n为2，最高位，不能为0，也就是有9个数可选，为了保证与高位不同，个位数可选9位数字 ，一共是81，因为求解[0,100),因此，81+10=91
- 如果n为3，最高位有9个，十位数是9个，个位数是8个，答案是`9*9*8+91=739`
- 一次类推

除了直接计算，采用dp保存前面的结果，进行缓存，这样在多次运算的时候就能直接得到结果



```python
class Solution:
    def countNumbersWithUniqueDigits(self, n):
        """
        :type n: int
        :rtype: int
        """
        dp = [1]

        if n == 0:
            return 1

        res = 10
        count = 9
        for i in range(2, n + 1):
            res += count * (11 - i)
            count *= 11 - i

        return res
```

​	这里简单的写一下