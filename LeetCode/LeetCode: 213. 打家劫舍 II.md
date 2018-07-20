# LeetCode: 213. 打家劫舍 II

[TOC]

## 1、题目描述



你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都**围成一圈，**这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你**在不触动警报装置的情况下，**能够偷窃到的最高金额。

**示例 1:**

```
输入: [2,3,2]
输出: 3
解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

**示例 2:**

```
输入: [1,2,3,1]
输出: 4
解释: 你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
```



## 2、解题思路

​	这道题目与打家劫舍不同的就在于，现在需要考虑租后一个房间和最后一个房间的情况，于是，我们可以将这个问题分解，分解成两个打家劫舍问题

举个例子，

```
输入：
[1 2 3]
我们分成两种情况，一种是冲第一个开始，到倒数第二个结束，一种是从第2个开始，到最后一个结束
[1 2]
[2 3]
分别用动态规划判断，找出最大的那个即可

```

```python
class Solution:
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        length = len(nums)

        if length == 0:
            return 0
        elif length == 1:
            return nums[0]

        temp = nums[1:]
        nums = nums[:-1]
        
        pre_pre1 = 0
        pre1 = nums[0]

        pre_pre2 = 0
        pre2 = temp[0]
        result1 = nums[0]
        result2 = temp[0]

        for i in range(1, len(nums)):
            cur1 = pre_pre1 + nums[i]
            cur2 = pre_pre2 + temp[i]
            result1 = max(cur1, pre1)
            result2 = max(cur2, pre2)

            pre_pre1 = pre1
            pre1 = result1

            pre_pre2 = pre2
            pre2 = result2

        return max(result1, result2)
```

