# LeetCode: 447. 回旋镖的数量

[TOC]



## 1、题目描述



给定平面上 *n* 对不同的点，“回旋镖” 是由点表示的元组 `(i, j, k)` ，其中 `i` 和 `j` 之间的距离和 `i` 和 `k` 之间的距离相等（**需要考虑元组的顺序**）。

找到所有回旋镖的数量。你可以假设 *n* 最大为 **500**，所有点的坐标在闭区间 **[-10000, 10000]** 中。

**示例:**

```
输入:
[[0,0],[1,0],[2,0]]

输出:
2

解释:
两个回旋镖为 [[1,0],[0,0],[2,0]] 和 [[1,0],[2,0],[0,0]]
```



## 2、解题思路



​	因为需要考虑顺序，所以，首先固定一个点，然后将其他所有点相对于该点的距离算出来

​	然后，如果数目大于2，表示能够构成回旋镖，在其中取两个，n(n-1),表示排列数



​	举个例子，这个就相当于排列数，首先固定了第一个元素，然后在剩下的找出来两个，排列

​	如果固定第二个元素，同样的道理



​	c语言实现有些麻烦，直接使用python

```python
class Solution:
    def numberOfBoomerangs(self, points):
        """
        :type points: List[List[int]]
        :rtype: int
        """
        result = 0
        for i in points:
            dis = {}
            for j in points:
                if i != j:
                    length = (i[0] - j[0])**2 + (i[1]-j[1])**2
                    dis[length] = dis.get(length,0)+1
            for val in dis.values():
                if val >=2:
                    result += val* (val-1)

        return result
```



