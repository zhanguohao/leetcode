# LeetCode: 39. 组合总和

[TOC]



## 1、题目描述

​	

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
```

**示例 2:**

```
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```



## 2、解题思路

​	使用递归的思路

​	首先，遇到一个数，我们就用target减掉这个数，判断一下，如果结果大于0，继续递归

​	如果等于0，将结果加入到结果集中

​	如果小于0，返回





```python
class Solution:
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        result = []

        self.findCombination(0, target, [], candidates, result)

        return result

    def findCombination(self, start, target, current, candidates, result):

        if target == 0:
            result.append(current)
        if target < 0:
            return

        for i in range(start, len(candidates)):
            self.findCombination(i, target - candidates[i], current + [candidates[i]], candidates, result)

```

