# LeetCode: 238. 除自身以外数组的乘积

[TOC]

## 1、题目描述



给定长度为 *n* 的整数数组 `nums`，其中 *n* > 1，返回输出数组 `output` ，其中 `output[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积。

**示例:**

```
输入: [1,2,3,4]
输出: [24,12,8,6]
```

**说明:** 请**不要使用除法，**且在 O(*n*) 时间复杂度内完成此题。

**进阶：**
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组**不被视为**额外空间。）

## 2、解题思路

​	如果不考虑时间复杂度和空间复杂度，题目很简单

​	考虑的话，就需要一些技巧

- 将前向乘积放到结果数组中
- 然后一面计算后向乘积，一面将结果放到数组中

```python
class Solution:
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        length = len(nums)
        result = [0] * length

        value = 1
        for i in range(length):
            result[i] = value
            value *= nums[i]

        value = 1
        for i in range(length - 1, -1, -1):
            result[i] = result[i] * value
            value *= nums[i]

        return result
```

