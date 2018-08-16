# LeetCode: 436. 寻找右区间

[TOC]

## 1、题目描述

给定一组区间，对于每一个区间 i，检查是否存在一个区间 j，它的起始点大于或等于区间 i 的终点，这可以称为 j 在 i 的“右侧”。

对于任何区间，你需要存储的满足条件的区间 j 的最小索引，这意味着区间 j 有最小的起始点可以使其成为“右侧”区间。如果区间 j 不存在，则将区间 i 存储为 -1。最后，你需要输出一个值为存储的区间值的数组。

**注意:**

1. 你可以假设区间的终点总是大于它的起始点。
2. 你可以假定这些区间都不具有相同的起始点。

**示例 1:**

```
输入: [ [1,2] ]
输出: [-1]

解释:集合中只有一个区间，所以输出-1。
```

**示例 2:**

```
输入: [ [3,4], [2,3], [1,2] ]
输出: [-1, 0, 1]

解释:对于[3,4]，没有满足条件的“右侧”区间。
对于[2,3]，区间[3,4]具有最小的“右”起点;
对于[1,2]，区间[2,3]具有最小的“右”起点。
```

**示例 3:**

```
输入: [ [1,4], [2,3], [3,4] ]
输出: [-1, 2, -1]

解释:对于区间[1,4]和[3,4]，没有满足条件的“右侧”区间。
对于[2,3]，区间[3,4]有最小的“右”起点。
```

## 2、解题思路

​	题目的大意很简单，就是对于当前区间，找出整个数组中在这个区间右面的那个区间的最小索引值

​	一般的思路是将左边界与索引值放入数组，然后将遍历每一个区间，判断大于当前区间的右边界的值的最小索引，这个过程可以通过二分法来判断，因为左边界不相同，所以判断也很简单

​	不过，直接考虑使用堆栈来做，更简单一些

- 将左边界与索引值作为元组放入数组中，然后建堆
- 将右边界与索引值作为元组放入数组中，然后建堆

接着，我们就遍历一遍，就能够直接得到结果了

```python
# Definition for an interval.
# class Interval:
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e

class Solution:
    def findRightInterval(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: List[int]
        """
        length = len(intervals)
        if length == 0:
            return []
        if length == 1:
            return [-1]

        ans = [0] * length

        left = [(value.start, index) for index, value in enumerate(intervals)]
        right = [(value.end, index) for index, value in enumerate(intervals)]

        heapq.heapify(left)
        heapq.heapify(right)

        left_count = 0
        left_temp = heapq.heappop(left)

        for i in range(length):
            right_temp = heapq.heappop(right)
            while left_count < length:
                if left_temp[0] >= right_temp[0]:
                    ans[right_temp[1]] = left_temp[1]
                    break
                else:
                    left_count += 1
                    if left_count < length:
                        left_temp = heapq.heappop(left)
            else:
                ans[right_temp[1]] = -1

        return ans
```

​	这样写比较简单，哈哈