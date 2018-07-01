# LeetCode: 849. 到最近的人的最大距离

[TOC]



## 1、题目描述





在一排座位（ `seats`）中，`1` 代表有人坐在座位上，`0` 代表座位上是空的。

至少有一个空座位，且至少有一人坐在座位上。

亚历克斯希望坐在一个能够使他与离他最近的人之间的距离达到最大化的座位上。

返回他到离他最近的人的最大距离。

**示例 1：**

```
输入：[1,0,0,0,1,0,1]
输出：2
解释：
如果亚历克斯坐在第二个空位（seats[2]）上，他到离他最近的人的距离为 2 。
如果亚历克斯坐在其它任何一个空位上，他到离他最近的人的距离为 1 。
因此，他到离他最近的人的最大距离是 2 。 
```

**示例 2：**

```
输入：[1,0,0,0]
输出：3
解释： 
如果亚历克斯坐在最后一个座位上，他离最近的人有 3 个座位远。
这是可能的最大距离，所以答案是 3 。
```

**提示：**

1. `1 <= seats.length <= 20000`
2. `seats` 中只含有 0 和 1，至少有一个 `0`，且至少有一个 `1`。



## 2、解题思路

​	

​	使用了基于字符串的想法，先把list变成字符串，然后通过1进行分割，第一个和最后一个单独判断，找出中间的连续最多的0的长度

​	

```python
class Solution:
    def maxDistToClosest(self, seats):
        """
        :type seats: List[int]
        :rtype: int
        """
        total = "".join([str(i) for i in seats]).split("1")
        left = total[0]
        right = total[len(total) - 1]

        if len(total[1:-1]) == 0:
            max_zeros = 0;
        else:
            max_zeros = len(max(total[1:-1], key=lambda x: len(x)))

        result = (max_zeros + 1) // 2
        if left != "":
            result = max(result, len(left))

        if right != "":
            result = max(result, len(right))
        return result
```

