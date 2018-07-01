# LeetCode: 40. 组合总和 II

[TOC]



## 1、题目描述



给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```



## 2、解题思路

​	

​	这个题目在前面的基础上，更改一下循环条件，并且判断是不是已经在结果集中就可以了

```python
class Solution:
    def combinationSum2(self, candidates, target):
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
            if sorted(current) not in result:
                result.append(sorted(current))
        if target < 0:
            return

        for i in range(start, len(candidates)):
            self.findCombination(i+1, target - candidates[i], current + [candidates[i]], candidates, result)

```

