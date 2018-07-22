# LeetCode: 228. 汇总区间

[TOC]

## 1、题目描述

给定一个无重复元素的有序整数数组，返回数组区间范围的汇总。

**示例 1:**

```
输入: [0,1,2,4,5,7]
输出: ["0->2","4->5","7"]
解释: 0,1,2 可组成一个连续的区间; 4,5 可组成一个连续的区间。
```

**示例 2:**

```
输入: [0,2,3,4,6,8,9]
输出: ["0","2->4","6","8->9"]
解释: 2,3,4 可组成一个连续的区间; 8,9 可组成一个连续的区间。
```

## 2、解题思路

​	题目比较简单，设置两个指针，一个指向区间首部，

​	另一个指向前面一个数，如果当前数减去前面的数之差不为1，就放到结果集中

```python
class Solution:
    def summaryRanges(self, nums):
        """
        :type nums: List[int]
        :rtype: List[str]
        """
        if len(nums) == 0:
            return []

        result = []
        first = nums[0]
        pre = nums[0] - 1

        for i in nums:

            if i - pre != 1:

                pre
                if first == pre:
                    result.append(str(first))
                else:
                    result.append(str(first) + "->" + str(pre))
                first = i

            pre = i

        if nums[-1] == first:
            result.append(str(first))
        else:
            result.append(str(first) + "->" + str(nums[-1]))

        return result
```



