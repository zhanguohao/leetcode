# LeetCode: 46. 全排列

[TOC]



## 1、题目描述



给定一个**没有重复**数字的序列，返回其所有可能的全排列。

**示例:**

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```



## 2、解题思路

​	其实可以这样构造，例如，我们可以取第一个，作为第一个数，取第二个作为第一个数，一直取，取到最后

​	然后递归，确定第二个数

​	一直到确定最后一个元素的时候，返回

```python
class Solution:
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        result = []
        self.dfs(nums, 0, result)
        return result

    # index表示现在要确定第几个数
    def dfs(self, nums, index, result):
        if index >= len(nums) - 1:
            result.append(nums)
            return
        for i in range(index, len(nums)):
            new_nums = nums[:]
            new_nums[index], new_nums[i] = new_nums[i], new_nums[index]
            self.dfs(new_nums, index + 1, result)
```

