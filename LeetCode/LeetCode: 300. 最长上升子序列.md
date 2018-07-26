# LeetCode: 300. 最长上升子序列

[TOC]

## 1、题目描述



给定一个无序的整数数组，找到其中最长上升子序列的长度。

**示例:**

```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

**说明:**

- 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
- 你算法的时间复杂度应该为 O(*n2*) 。

**进阶:** 你能将算法的时间复杂度降低到 O(*n* log *n*) 吗?



## 2、解题思路

### 2.1 动态规划

​	使用动态规划，建立一个数组，当前位置上的数表示从前向后扫描的过程中，最大的子数组值

```python
class Solution:
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        
        dp = [1] * len(nums)
        res = 1
        for i in range(len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], 1 + dp[j])

            res = max(res, dp[i])

        return res
```



### 2.2 数组法

​	我们假设这样的问题，假如我们挑出来最长的上升子序列

- 如果这个元素小于数组首部元素，将首部元素替换掉

- 如果该元素大于数组尾部元素，添加到尾部

- 如果遍历到的新元素比ends数组首元素大，比尾元素小时，此时用二分查找法找到第一个不小于此新元素的位置，覆盖掉位置的原来的数字，

```python
class Solution:
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0

        buff = [nums[0]]

        for i in range(1, len(nums)):
            if nums[i] < buff[0]:
                buff[0] = nums[i]
            elif nums[i] > buff[-1]:
                buff.append(nums[i])
            else:

                left = 0
                right = len(buff) - 1

                while left < right:
                    mid = (left + right) // 2
                    if nums[i] > buff[mid]:
                        left = mid + 1
                    else:
                        right = mid
                buff[right] = nums[i]
        return len(buff)
```



​	