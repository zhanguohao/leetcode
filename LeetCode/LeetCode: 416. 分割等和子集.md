# LeetCode: 416. 分割等和子集

[TOC]

## 1、题目描述

给定一个**只包含正整数**的**非空**数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**注意:**

1. 每个数组中的元素不会超过 100
2. 数组的大小不会超过 200

**示例 1:**

```
输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
```

 

**示例 2:**

```
输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集.
```

## 2、解题思路

​	首先来分析一下，什么条件是满足的，也就是说我们首先从里面找出加起来和等于总和一半的子集，只要子集存在，就返回True

​	若子数组总和加起来是奇数，那么肯定是False

​	

**思路一：**

​	使用递归求解，对每一个元素，我们有两种选择，加上或者不加，这样就有了两个分支，当和已经大于总和一半的时候，直接返回False

​	如果小于，分两种情况，一种是加上当前数，另一个是不加当前数，然后进行判断

```python
class Solution:
    def canPartition(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        if len(nums) < 2:
            return False

        total = sum(nums)
        if total % 2 != 0:
            return False

        return self.dfs(0, nums, 0, total // 2)

    def dfs(self, index, nums, cur, target):
        if index >= len(nums):
            return False
        if cur == target:
            return True
        elif cur > target:
            return False
        else:
            return self.dfs(index + 1, nums, cur, target) or self.dfs(index + 1, nums, cur + nums[index], target)
```

​	超出时间限制了，这个递归的思路是很简单的，

**思路二：**

​	动态规划法

​	建立一个二维数组，横坐标表示前面i个元素，纵坐标表示能够得到和j



例如

```
[1, 5, 11, 5]
一共4个元素，那么i取值0，1，2，3，4，分别表示取0个元素，1个元素，
然后总和是22，取一半就是11，因此，纵坐标取值0-11，
然后我们就得到前面i个元素是不是能够得到j这个值
```

```python
dp[i][j] = dp[i-1][j] or dp[i-1][j-nums[i]]
```

​	上面是状态转换方程

​	二维的数组状态很多，然后分析一下，实际上，对于每一个j来说，只要上一层中，不管取几个元素，只要能够得到，我们就设置为True，如果都不能得到，就设置为Fasle

​	那么，我们就能够根据i一开始的取值，初始化j

​	对应的i所在的位置，都设置为True

```
dp[j] = dp[j] or dp[j-nums[i]]
```

​	于是，状态转换变成了一维的，节省了空间，将低了计算量

```python
class Solution:
    def canPartition(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        
        if len(nums) < 2:
            return False

        total = sum(nums)
        if total % 2 != 0:
            return False

        target = total // 2

        dp = [False] * (target + 1)

        dp[0] = True

        for i in nums:
            for j in range(target, 0, -1):
                if dp[j]:
                    break
                else:
                    if j >= i:
                        dp[j] = dp[j] or dp[j - i]
        return dp[-1]
```

