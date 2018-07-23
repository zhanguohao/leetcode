# LeetCode: 229. 求众数 II

[TOC]



## 1、题目描述



给定一个大小为 *n* 的数组，找出其中所有出现超过 `⌊ n/3 ⌋` 次的元素。

**说明:** 要求算法的时间复杂度为 O(n)，空间复杂度为 O(1)。

**示例 1:**

```
输入: [3,2,3]
输出: [3]
```

**示例 2:**

```
输入: [1,1,1,3,3,2,2,2]
输出: [1,2]
```



## 2、解题思路

​	在前面已经做过求众数，之前是超过一半，现在是超过`⌊ n/3 ⌋` ，那么最多就会有2个众数

​	同样的，使用摩尔投票法，选出两个候选众数

​	然后判断他们出现的次数是不是超过限定值



```python
class Solution:
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if not nums:
            return []
        
        m = nums[0] - 1
        n = nums[0] - 1

        count_m = 0
        count_n = 0
        for i in nums:
            if i == m:
                count_m += 1
            elif i == n:
                count_n += 1
            elif count_m <= 0:
                m = i
                count_m = 1
            elif count_n <= 0:
                n = i
                count_n = 1
            else:
                count_m -= 1
                count_n -= 1

        count_m = 0
        count_n = 0
        for i in nums:
            if i == m:
                count_m += 1
            elif i == n:
                count_n += 1

        result = []
        if count_m > len(nums) // 3:
            result.append(m)
        if count_n > len(nums) // 3:
            result.append(n)
        return result
```

