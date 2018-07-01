# LeetCode: 18.四数之和

[TOC]

## 1、题目描述



​	给定一个包含 *n* 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 *a，**b，c* 和 *d* ，使得 *a* + *b* + *c* + *d* 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

**注意：**

答案中不可以包含重复的四元组。

**示例：**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```









## 2、解题思路

​	借助前面的3数之和，进行求解

​	3数之和借助2数之和进行求解

​	所以，实际上也可以借助递归进行求解，这样不管是几个数之和，都能求解出来

```python
class Solution:
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        if len(nums) < 4:
            return []

        sorted_nums = sorted(nums)
        result = []
        
        for i in range(len(nums)):
            temp = self.threeSum(sorted_nums[i + 1:], target - sorted_nums[i])
            if temp != None:
                for k in temp:
                    if [sorted_nums[i]]+k not in result:
                        result.append([sorted_nums[i]]+ k)
        return result

    def threeSum(self, nums, target):
        if len(nums) < 3:
            return []

        result = []
        count_equal_one = nums

        for i in range(len(count_equal_one)):
            temp = {}
            for j in range(i + 1, len(count_equal_one)):
                if count_equal_one[j] in temp:
                    result.append([count_equal_one[i], count_equal_one[j], temp[count_equal_one[j]]])
                else:
                    temp[target - count_equal_one[i] - count_equal_one[j]] = count_equal_one[j]

        return result
```

