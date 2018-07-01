# LeetCode: 47. 全排列 II

[TOC]



## 1、题目描述



给定一个可包含重复数字的序列，返回所有不重复的全排列。

**示例:**

```
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```





## 2、解题思路

​	在前面46题全排列的基础上，使用set无重复做存储，就得到结果了



```python
class Solution:
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        temp_result = set()
        self.dfs(nums, 0, temp_result)
        return [list(v) for v in temp_result]

    # index表示现在要确定第几个数
    def dfs(self, nums, index, temp_result):
        if index >= len(nums) - 1:
            temp_result.add(tuple(nums))
            return
        for i in range(index, len(nums)):
            new_nums = nums[:]
            if index != i and new_nums[index] == new_nums[i]:
                continue
            new_nums[index], new_nums[i] = new_nums[i], new_nums[index]
            self.dfs(new_nums, index + 1, temp_result)
```

