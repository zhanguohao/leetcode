# LeetCode: 166. 分数到小数

[TOC]



## 1、题目描述



给定两个整数，分别表示分数的分子 numerator 和分母 denominator，以字符串形式返回小数。

如果小数部分为循环小数，则将循环的部分括在括号内。

**示例 1:**

```
输入: numerator = 1, denominator = 2
输出: "0.5"
```

**示例 2:**

```
输入: numerator = 2, denominator = 1
输出: "2"
```

**示例 3:**

```
输入: numerator = 2, denominator = 3
输出: "0.(6)"
```



## 2、解题思路

​	再找思路的过程中，反向思考，如果是无限循环小数，如何变成分数呢？

```
步骤1.将无限循环小数分为2个部分,0.3454545...45为例,将其分0.3+0.04545...45这2个部分.
步骤2.将这2个部分分别化成分数,0.3=3/10,
0.0454545...45的划分方法.先设它为a,那么就有：
10a=0.454545...45
1000a=45.4545.45
1000a-10a=45
990a=45
a=45/990=1/22
所以0.0454545...45=1/22
步骤3.再将2个部分相加就得到该无限循环小数化成分数的结果了
3/10+1/22=66/220+10/220=76/220=19/55
所以0.3454545...45=19/55
```

​	这个题目可以这样做，首先将数据分成两部分，整数部分和小数部分，

- 建立一个哈希，缓存余数
- 对小数部分进行判断，是不是循环的，如果余数不为0，表示要继续计算下去
- 如果余数出现过了，表示这个就是循环的，当前的数字不需要加入结果集中



有一点需要注意，符号的问题，先把结果的正负号保存下来，然后计算的都用正数

```python
class Solution:
    def fractionToDecimal(self, numerator, denominator):
        """
        :type numerator: int
        :type denominator: int
        :rtype: str
        """
        
        sign = 1
        if (numerator > 0 and denominator < 0) or (numerator < 0 and denominator > 0):
            sign = -1

        numerator = abs(numerator)
        denominator = abs(denominator)

        integer_part = numerator // denominator
        remainder = numerator % denominator

        if sign == 1:
            result = ""
        else:
            result = "-"

        if remainder == 0:
            return result + str(integer_part)

        remainder_hash = {}

        decimal_cache = []
        is_cycle = False
        cycle_index = 0

        while remainder != 0:
            temp = remainder = remainder
            cur_num = remainder * 10 // denominator
            remainder = remainder * 10 % denominator
            if remainder_hash.get(temp) is None:
                remainder_hash[temp] = cycle_index
                decimal_cache.append(cur_num)
                cycle_index += 1
            else:
                cycle_index = remainder_hash[temp]
                is_cycle = True
                break

        if not is_cycle:
            result += str(integer_part) + "." + "".join([str(i) for i in decimal_cache])
        else:
            result += str(integer_part) + "." + "".join([str(i) for i in decimal_cache[:cycle_index]]) + "(" + "".join(
                [str(i) for i in decimal_cache[cycle_index:]]) + ")"

        return result
```

