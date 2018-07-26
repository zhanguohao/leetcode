# LeetCode: 287. 寻找重复数

[TOC]



## 1、题目描述

给定一个包含 *n* + 1 个整数的数组 *nums*，其数字都在 1 到 *n* 之间（包括 1 和 *n*），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

**示例 1:**

```
输入: [1,3,4,2,2]
输出: 2
```

**示例 2:**

```
输入: [3,1,3,4,2]
输出: 3
```

**说明：**

1. **不能**更改原数组（假设数组是只读的）。
2. 只能使用额外的 *O*(1) 的空间。
3. 时间复杂度小于 *O*(*n*2) 。
4. 数组中只有一个重复的数字，但它可能不止重复出现一次。



## 2、解题思路

​	采用二分法，判断，因为数字只存在1-n，所以找出重复的数，就利用小于等于当前数的数字数量是不是大于当前数，如果是，表示重复的小于当前数，否则表示重复的大于当前数

```python
class Solution:
    def findDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        left = 0
        right = len(nums) - 1

        while left <= right:
            mid = (left + right) // 2
            count = 0
            for i in nums:
                if i <= mid:
                    count += 1

            if count > mid:
                right = mid - 1
            else:
                left = mid + 1

        return left
```



​	还可以用字典，直接扫描一遍，保存数字出现的次数

