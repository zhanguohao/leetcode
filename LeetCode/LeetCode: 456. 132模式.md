# LeetCode: 456. 132模式

[TOC]

## 1、题目描述

给定一个整数序列：a1, a2, ..., an，一个132模式的子序列 a**i**, a**j**, a**k** 被定义为：当 **i** < **j** < **k** 时，a**i** < a**k** < a**j**。设计一个算法，当给定有 n 个数字的序列时，验证这个序列中是否含有132模式的子序列。

**注意：**n 的值小于15000。

**示例1:**

```
输入: [1, 2, 3, 4]

输出: False

解释: 序列中不存在132模式的子序列。
```

**示例 2:**

```
输入: [3, 1, 4, 2]

输出: True

解释: 序列中有 1 个132模式的子序列： [1, 4, 2].
```

**示例 3:**

```
输入: [-1, 3, 2, 0]

输出: True

解释: 序列中有 3 个132模式的的子序列: [-1, 3, 2], [-1, 3, 0] 和 [-1, 2, 0].
```

## 2、解题思路

	序列从前向后扫描，找出3个数字，符合题意

```
	不过，一般的思路，直接进行三重循环，这个时间复杂度太高了
	需要换种思路
```

- 对每个节点，我们扫描一遍，能够判断对于当前节点来讲，前方最小的节点值是多少
- 然后，将栈中小于等于这个最小值的数字出栈
  - 判断栈中的数字，如果小于当前数字，返回True
  - 如果不能，放入栈中，继续判断

```python
class Solution:
    def find132pattern(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        length = len(nums)
        if length < 3:
            return False

        mins = [0] * length
        min_value = nums[0]
        for i in range(length):
            min_value = min(min_value, nums[i])
            mins[i] = min_value

        stack = []

        for i in range(length - 1, -1, -1):
            if nums[i] > mins[i]:
                while stack and stack[-1] <= mins[i]:
                    stack.pop()

                if stack and stack[-1] < nums[i]:
                    return True

                stack.append(nums[i])

        return False
```

