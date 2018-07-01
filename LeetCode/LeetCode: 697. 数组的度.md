# LeetCode: 697. 数组的度

[TOC]



## 1、题目描述





给定一个非空且只包含非负数的整数数组 `nums`, 数组的度的定义是指数组里任一元素出现频数的最大值。

你的任务是找到与 `nums` 拥有相同大小的度的最短连续子数组，返回其长度。

**示例 1:**

```
输入: [1, 2, 2, 3, 1]
输出: 2
解释: 
输入数组的度是2，因为元素1和2的出现频数最大，均为2.
连续子数组里面拥有相同度的有如下所示:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
最短连续子数组[2, 2]的长度为2，所以返回2.
```

**示例 2:**

```
输入: [1,2,2,3,1,4,2]
输出: 6
```

**注意:**

- `nums.length` 在1到50,000区间范围内。
- `nums[i]` 是一个在0到49,999范围内的整数。



## 2、解题思路

​	首先找出最大的度，然后从数组的左面和右面找到这个数字第一次出现的位置，返回这两个位置之差；



```python
class Solution:
    def findShortestSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        result = 50000000
        num_dict = {}
        count = 0
        index = 0

        for i in nums:
            if num_dict.get(i) is None:
                num_dict[i] = [1, index, index]

            else:
                num_dict[i][0] += 1
                num_dict[i][2] = index
            index += 1
        count = max([v[0] for v in num_dict.values()])

        for k, v in num_dict.items():
            if v[0] == count:
                result = min(result, v[2]-v[1]+1)

        return result
```

