# LeetCode: 34. 搜索范围

[TOC]



## 1、题目描述





给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 *O*(log *n*) 级别。

如果数组中不存在目标值，返回 `[-1, -1]`。

**示例 1:**

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
```

**示例 2:**

```
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```



## 2、解题思路

​	首先，我么能使用二分法，找到这个元素在数组中的位置，如果找不到，就返回[-1,-1]

​	如果找得到，表示继续使用二分法，找出最小，和最大的下标



```python
class Solution:
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        left = 0
        right = len(nums) - 1
        middle = (left + right) // 2
        result = [-1, -1]
        while left <= right:
            if nums[middle] == target:
                result[0] = self.findMinIndex(nums, middle, target)
                result[1] = self.findMaxIndex(nums, middle, target)

            if nums[middle] < target:
                left = middle + 1

            else:
                right = middle - 1
            middle = (left + right) // 2
        return result

    def findMinIndex(self, nums, current, target):
        left = 0
        right = current

        middle = (left + right) // 2
        while left <= right:
            if nums[middle] == target and middle == 0:
                return middle
            if nums[middle] == target and middle > 0 and nums[middle - 1] != target:
                return middle

            if nums[middle] < target:
                left = middle + 1
            else:
                right = middle - 1
            middle = (left + right) // 2
        return current

    def findMaxIndex(self, nums, current, target):
        left = current
        right = len(nums) - 1

        middle = (left + right) // 2
        while left <= right:
            if nums[middle] == target and middle == len(nums) - 1:
                return middle
            if nums[middle] == target and middle > 0 and nums[middle + 1] != target:
                return middle

            if nums[middle] > target:
                right = middle - 1
            else:
                left = middle + 1
            middle = (left + right) // 2
        return current

```



