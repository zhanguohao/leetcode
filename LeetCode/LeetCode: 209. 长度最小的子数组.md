# LeetCode: 209. 长度最小的子数组

[TOC]



## 1、题目描述



给定一个含有 **n** 个正整数的数组和一个正整数 **s ，**找出该数组中满足其和 **≥ s** 的长度最小的子数组**。**如果不存在符合条件的子数组，返回 0。

**示例:** 

```
输入: s = 7, nums = [2,3,1,2,4,3]
输出: 2
解释: 子数组 [4,3] 是该条件下的长度最小的子数组。
```

**进阶:**

如果你已经完成了*O*(*n*) 时间复杂度的解法, 请尝试 *O*(*n* log *n*) 时间复杂度的解法。



## 2、解题思路

​	设置双指针，如果小于s，就将right加一，字数组之和加上新的数字

​	如果大于s，就更新result，并且减去left指向的值，left加一



```python
class Solution:
    def minSubArrayLen(self, s, nums):
        """
        :type s: int
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) <= 0:
            return 0

        length = len(nums)

        result = length + 1
        num_sums = nums[0]
        left = 0
        right = 1

        while left < length:
            if num_sums >= s:
                result = min(result, right - left)
                num_sums -= nums[left]
                left += 1
            else:
                if right < length:
                    num_sums += nums[right]
                    right += 1
                else:
                    break

        if result == length + 1:
            result = 0

        return result
```

