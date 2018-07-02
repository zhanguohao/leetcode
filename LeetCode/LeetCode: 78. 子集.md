# LeetCode: 78. 子集

[TOC]

## 1、题目描述



给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```



## 2、解题思路

​	看到这个题目，直接想到的解题就是利用求解组合数，从求解0个，到求解n个，然后放到结果数组中即可



```python
class Solution:
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        result = []

        for i in range(len(nums)+1):
            result += [list(v) for v in itertools.combinations(nums, i)]

        return result
```



