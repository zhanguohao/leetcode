# LeetCode: 386. 字典序排数

[TOC]



## 1、题目描述

给定一个整数 *n*, 返回从 *1* 到 *n* 的字典顺序。

例如，

给定 *n* =1 3，返回 [1,10,11,12,13,2,3,4,5,6,7,8,9] 。

请尽可能的优化算法的时间复杂度和空间复杂度。 输入的数据 *n* 小于等于 5,000,000。



## 2、解题思路

​	可以直接排序，就是字符串形式的顺序

```python
class Solution:
    def lexicalOrder(self, n):
        """
        :type n: int
        :rtype: List[int]
        """
        return sorted(range(1, n+1), key=lambda x: str(x))
```

​	使用深度优先搜索来做

```python
class Solution:
    def lexicalOrder(self, n):
        """
        :type n: int
        :rtype: List[int]
        """
        res = []

        for i in range(1, 10):
            self.dfs(i, n, res)
        return res

    def dfs(self, start, n, res):

        if start > n:
            return
        else:
            res.append(start)
            for i in range(10):
                if (10 * start + i) > n:
                    return
                self.dfs(10 * start + i, n, res)
```



