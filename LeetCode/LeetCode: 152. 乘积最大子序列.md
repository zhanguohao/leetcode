# LeetCode: 152. 乘积最大子序列

[TOC]

## 1、题目描述



给定一个整数数组 `nums` ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。

**示例 1:**

```
输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**示例 2:**

```
输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```



## 2、解题思路

​	看到这道题，首先就想到动态规划，难点就是需要确认一下转换方程是什么

​	因为数字有正有负，如果要判断当前节点之前，最大的子序列的乘积，可以这样做

- 判断不包含当前节点的最大的乘积，包含前面一个节点的最大乘积，最小乘积，分别乘以当前数，找出其中最大的
-  更新包含当前节点的最大乘积，最小乘积



​	状态转换方程如下：
$$
dp[i] = max(dp[i-1],prevMax * nums[i], prevMin*nums[i],nums[i])
\\
prevMax=max(prevMax*nums[i], prevMin*nums[i],nums[i])
\\
prevMin=min(prevMax*nums[i], prevMin*nums[i],nums[i])
$$

```python
class Solution:
    def maxProduct(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        length = len(nums)
        dp = [0] * length

        dp[0] = nums[0]
        prev_max = nums[0]
        prev_min = nums[0]

        for i in range(1, length):
            
            temp_max = prev_max * nums[i]
            temp_min = prev_min * nums[i]
            dp[i] = max(dp[i - 1], temp_max, temp_min, nums[i])
            prev_max = max(temp_max, temp_min, nums[i])
            prev_min = min(temp_max, temp_min, nums[i])

        return dp[-1]
```

