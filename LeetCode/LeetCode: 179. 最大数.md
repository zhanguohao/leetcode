# LeetCode: 179. 最大数

[TOC]



## 1、题目描述

给定一组非负整数，重新排列它们的顺序使之组成一个最大的整数。

**示例 1:**

```
输入: [10,2]
输出: 210
```

**示例 2:**

```
输入: [3,30,34,5,9]
输出: 9534330
```

**说明:** 输出结果可能非常大，所以你需要返回一个字符串而不是整数。



## 2、解题思路

​	基本思路首先是将所有数字进行排序，排序规则自定义，如何取定两个数的顺序呢，就是如果数字1放到数字2之前，得到的数字比起数字2放到数字1之前更大，放回整数

​	python3中，sorted函数取消了对cmp的支持，不过在functiontool里面还能转换使用

​	这道题是求解最大的数，如果是求解最小的数，只需要将compare的比较反转即可

```python
import functools
class Solution(object):
    def largestNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: str
        """
        cmpkey = functools.cmp_to_key(self.compare)
        nums.sort(key=cmpkey)

        if str(nums[0])[0] == '0':
            return '0'

        return "".join([str(v) for v in nums])

    def compare(self, num1, num2):
        return int(str(num2) + str(num1)) - int(str(num1) + str(num2))
```



​	下面是python2的解法

```python
class Solution(object):
    def largestNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: str
        """

        nums.sort(cmp = self.compare)

        if str(nums[0])[0] == '0':
            return '0'

        return "".join([str(v) for v in nums])

    def compare(self, num1, num2):
        return int(str(num2) + str(num1)) - int(str(num1) + str(num2))
```

