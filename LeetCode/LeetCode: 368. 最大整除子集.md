# LeetCode: 368. 最大整除子集

[TOC]

## 1、题目描述

给出一个由**无重复的**正整数组成的集合, 找出其中最大的整除子集, 子集中任意一对 (Si, Sj) 都要满足: Si % Sj = 0 或 Sj % Si = 0。

如果有多个目标子集，返回其中任何一个均可。

**示例 1:**

```
集合: [1,2,3]

结果: [1,2] (当然, [1,3] 也正确)
```

 **示例 2:**

```
集合: [1,2,4,8]

结果: [1,2,4,8]
```

**致谢：**
特别感谢 [@Stomach_ache](https://leetcode.com/stomach_ache) 添加这道题并创建所有测试用例。



## 2、解题思路

​	使用动态规划来做

- 首先，对数组进行排序
- 然后对于当前数字，先前寻找，能够整除的最大的数目

```
dp[i] = max(dp[i], dp[j]+1)
```

举个例子

```
[1,2,3]
建立一个数组，存放当前点最大的整除序列数目
在建立一个数组，保存当前节点最大的整除序列是由前面哪一个点增加得来的
```

```
初始化为1，表示所有节点能够被自己整除
[1 1 1]
初始化为自己的下标，表示还没有通过前面的那个节点，得到最大的整除序列
[0 1 2]

首先是第一个点
1，前面没有节点了，不用判断
然后是2，判断能被1整除，更新dp，下标数组
[1 2 1]
[0 0 2]
然后是3，3可以被1整除，更新dp，下标数组
[1 2 2]
[0 0 0]
于是，我们就通过前向数组，就能得到整除的那个序列
现在最大的就是2，从2这个点开始
2，1
到前向节点与当前节点是一个截止
```

```python
class Solution:
    def largestDivisibleSubset(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if not nums:
            return []
        
        length = len(nums)

        dp = [1] * length
        pre = [i for i in range(length)]

        nums.sort()
        max_length = 1
        max_index = 0
        for i in range(length):
            for j in range(i):
                if nums[i] % nums[j] == 0 and dp[i] < dp[j] + 1:
                    dp[i] = dp[j] + 1
                    pre[i] = j

            if dp[i] > max_length:
                max_length = dp[i]
                max_index = i

        result = []

        while max_index != pre[max_index]:
            result.append(nums[max_index])
            max_index = pre[max_index]

        result.append(nums[max_index])
        
        return result
```

