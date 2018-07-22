# LeetCode: 223. 矩形面积

[TOC]



## 1、题目描述



在**二维**平面上计算出两个**由直线构成的**矩形重叠后形成的总面积。

每个矩形由其左下顶点和右上顶点坐标表示，如图所示。

![Rectangle Area](https://leetcode-cn.com/static/images/problemset/rectangle_area.png)

**示例:**

**输入:** -3, 0, 3, 4, 0, -1, 9, 2 **输出:** 45

**说明:** 假设矩形面积不会超出 **int** 的范围。



## 2、解题思路

​	基本思路比较简单，分成两步：

- 判断有没有相交
- 如果相交，对横纵坐标分别排序，找出中间的两个，然后得到新的相交矩形
- 然后用总的面积减去相交面积

```python
class Solution:
    def computeArea(self, A, B, C, D, E, F, G, H):
        """
        :type A: int
        :type B: int
        :type C: int
        :type D: int
        :type E: int
        :type F: int
        :type G: int
        :type H: int
        :rtype: int
        """
        total = (C - A) * (D - B) + (G - E) * (H - F)

        if A >= G or C <= E or B >= H or D <= F:
            return total

        temp_row = sorted([A, C, E, G])[1:-1]
        temp_col = sorted([B, D, F, H])[1:-1]

        intersection = (temp_row[1] - temp_row[0]) * (temp_col[1] - temp_col[0])
        return total - intersection
```



