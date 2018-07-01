# LeetCode: 812. 最大三角形面积

[TOC]



## 1、题目描述



给定包含多个点的集合，从其中取三个点组成三角形，返回能组成的最大三角形的面积。

```
示例:
输入: points = [[0,0],[0,1],[1,0],[0,2],[2,0]]
输出: 2
解释: 
这五个点如下图所示。组成的橙色三角形是最大的，面积为2。
```

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/04/1027.png)

**注意:** 

- `3 <= points.length <= 50`.
- 不存在重复的点。
-  `-50 <= points[i][j] <= 50`.
- 结果误差值在 `10^-6` 以内都认为是正确答案。



## 2、解题思路

​	这个题目的难点在于要知道如何利用三个点的坐标求解面积，

​	有时间可以看看有限元分析



已知三个点坐标求 三角形面积

```
设A(x1,y1),B(x2,y2),C(x3,y3) 
由A-->B-->C-->A 按逆时针方向转。(行列式书写要求） 
设三角形的面积为S 
则S=（1/2)*(下面行列式） 
|x1 y1 1| 
|x2 y2 1| 
|x3 y3 1| 
S=(1/2)*(x1y2*1+x2y3*1+x3y1*1-x1y3*1-x2y1*1-x3y2*1) 
即用三角形的三个顶点坐标求其面积的公式为: 
S=(1/2)*(x1y2+x2y3+x3y1-x1y3-x2y1-x3y2)
```

 

```python
class Solution:
    def largestTriangleArea(self, points):
        """
        :type points: List[List[int]]
        :rtype: float
        """
        return max([self.getArea(point1, point2, point3) for point1, point2, point3 in itertools.combinations(points, 3)])

    def getArea(self, point1, point2, point3):
        (x1, y1), (x2, y2), (x3, y3) = point1, point2, point3
        return 0.5 * abs(x1 * (y2 - y3) + x2 * (y3 - y1) + x3 * (y1 - y2))
```



