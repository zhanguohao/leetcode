# LeetCode: 307. 区域和检索 - 数组可修改

[TOC]



## 1、题目描述

给定一个整数数组  *nums*，求出数组从索引 *i* 到 *j*  (*i* ≤ *j*) 范围内元素的总和，包含 *i,  j* 两点。

*update(i, val)* 函数可以通过将下标为 *i* 的数值更新为 *val*，从而对数列进行修改。

**示例:**

```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```

**说明:**

1. 数组仅可以在 *update* 函数下进行修改。
2. 你可以假设 *update* 函数与 *sumRange* 函数的调用次数是均匀分布的。

## 2、解题思路



​	先来理清一下思路，假设不能修改，我们会怎么做呢？

​	建立一个缓冲数组，然后计算从第一个到当前值的和值，当我们要得到对应段的时候，做一下差值就可以了

树状数组一般适用于三类问题：

1，修改一个点求一个区间

2，修改一个区间求一个点

3，求逆序列对

使用Binary Indexed Tree来做，更加简单

利用的是数的性质，参见后文的blog



```python
class NumArray:

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        
        self.nums = nums

        self.buff = [0] * (len(nums) + 1)
        for i in range(1, len(nums) + 1):
            temp = nums[i - 1]
            while i < (len(nums) + 1):
                self.buff[i] += temp
                i += i & (-i)

    def update(self, i, val):
        """
        :type i: int
        :type val: int
        :rtype: void
        """
        index = i + 1
        diff = val - self.nums[i]
        while index < len(self.buff):
            self.buff[index] += diff
            index += index & (-index)

        self.nums[i] = val

    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        return self.getSum(j + 1) - self.getSum(i)

    def getSum(self, index):
        res = 0

        while index > 0:
            res += self.buff[index]
            index -= index & (-index)

        return res

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```

​	代码超过了100%的，哈哈

​	

[线段树]: https://baike.baidu.com/item/线段树/10983506?fr=aladdin
[blog]: https://blog.csdn.net/aishangyutian12/article/details/52079059

