# LeetCode: 698. 划分为k个相等的子集

[TOC]

## 1、题目描述

给定一个整数数组  `nums` 和一个正整数 `k`，找出是否有可能把这个数组分成 `k` 个非空子集，其总和都相等。

**示例 1：**

```
输入： nums = [4, 3, 2, 3, 5, 2, 1], k = 4
输出： True
说明： 有可能将其分成 4 个子集（5），（1,4），（2,3），（2,3）等于总和。
```

 

**注意:**

- `1 <= k <= len(nums) <= 16`
- `0 < nums[i] < 10000`

## 2、解题思路

- 首先，判断数组长度是不是大于k，如果小于k，直接返回False
- 判断总和能不能被4整除，如果不可以，直接返回False
- 然后调用递归函数
- 判断当前和是不是target，并且k是1，直接返回True
- 如果k不是1，但是当前和是target，k-1，然后继续判断
- 如果不是target，这时候，遍历所有索引后面的元素，找出可能的值，加上判断是不是小于等于target，并且没有被访问过
- 如果这种方案无法得到结果，当前值设置为未访问过，继续判断



```python
class Solution:
    def canPartitionKSubsets(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        if len(nums) < k:
            return False
        total = sum(nums)
        if total % k != 0:
            return False

        target = total // k

        visited = [0] * len(nums)

        def dfs(k, index, cur_sum):
            if k == 1 and cur_sum == target:
                return True

            if cur_sum == target:
                return dfs(k - 1, 0, 0)
            else:
                for i in range(index, len(nums)):
                    if nums[i] + cur_sum <= target and not visited[i]:
                        visited[i] = 1
                        if dfs(k, i + 1, nums[i] + cur_sum):
                            return True
                        visited[i] = 0
                return False

        return dfs(k, 0, 0)
```

