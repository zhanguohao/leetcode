# LeetCode: 365. 水壶问题

[TOC]

## 1、题目描述

有两个容量分别为 *x*升 和 *y*升 的水壶以及无限多的水。请判断能否通过使用这两个水壶，从而可以得到恰好 *z*升 的水？

如果可以，最后请用以上水壶中的一或两个来盛放取得的 *z升* 水。

你允许：

- 装满任意一个水壶
- 清空任意一个水壶
- 从一个水壶向另外一个水壶倒水，直到装满或者倒空

**示例1:** (From the famous [*"Die Hard"* example](https://www.youtube.com/watch?v=BVtQNK_ZUJg))

```
输入: x = 3, y = 5, z = 4
输出: True
```

**示例2:**

```
输入: x = 2, y = 6, z = 5
输出: False
```

**致谢:**
感谢 [@vinod23](https://discuss.leetcode.com/user/vinod23) 添加这个问题并创建所有测试用例。



## 2、解题思路

​	这个题目是一个数学问题，用贝祖定理可以解决

```
贝祖定理：
	贝祖定理是代数几何中一个定理，其内容是若设a,b是整数，则存在整数x,y，使得ax+by=gcd（a,b），(a,b)代表最大公因数，则设a,b是不全为零的整数，则存在整数x,y，使得ax+by=(a，b)。
```

- z <= x+y
- z == x True
- z == y True
- z == x+y True
- z == abs(x-y) True
- 如果z%(gcd) == 0, 也就是说，z肯定能够使用x，y的组合构成

求解最大公约数，使用辗转相除法

```python
class Solution:
    def canMeasureWater(self, x, y, z):
        """
        :type x: int
        :type y: int
        :type z: int
        :rtype: bool
        """
        def gcd(a, b):
            if b > a:
                a, b = b, a

            while b != 0:
                a, b = b, a % b
            return a

        if z > x + y:
            return False
        if z == x + y or z == x or z == y or z == abs(x - y):
            return True
        return z % gcd(x, y) == 0
        
```

​	虽然通过了，不过这个基本上是一个数学题，这样并不能很好地体现计算机的解题思路

​	如果是纯计算机解题，那么