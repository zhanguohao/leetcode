# LeetCode: 77. 组合

[TOC]



## 1、题目描述



给定两个整数 *n* 和 *k*，返回 1 ... *n* 中所有可能的 *k* 个数的组合。

**示例:**

```
输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```





## 2、解题思路

​	直接使用最简单的思路的话，也就是使用python内置的itertools，直接求解

​	如果不使用的话，就需要不断的迭代，求解

```python
class Solution:
    def combine(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: List[List[int]]
        """
        temp = [i for i in range(1, n + 1)]

        result = itertools.combinations(temp, k)
        return [ list(v) for v in result]

```

