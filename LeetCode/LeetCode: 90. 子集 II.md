# LeetCode: 90. 子集 II

[TOC]



## 1、题目描述



给定一个可能包含重复元素的整数数组 **\*nums***，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```



## 2、解题思路

​	实际上，这道题与前面的子集并没有什么区别，只需要判断一下是不是重复的即可

​	借助实现的求解组合的函数

```python
class Solution:
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        hash_dict = {}

        for i in range(len(nums) + 1):
            for k in itertools.combinations(nums, i):
                hash_dict[tuple(sorted(k))] = 1

        return [list(v) for v in hash_dict.keys()]
        
```

