# LeetCode: 435. 无重叠区间

[TOC]

## 1、题目描述

给定一个区间的集合，找到需要移除区间的最小数量，使剩余区间互不重叠。

**注意:**

1. 可以认为区间的终点总是大于它的起点。
2. 区间 [1,2] 和 [2,3] 的边界相互“接触”，但没有相互重叠。

**示例 1:**

```
输入: [ [1,2], [2,3], [3,4], [1,3] ]

输出: 1

解释: 移除 [1,3] 后，剩下的区间没有重叠。
```

**示例 2:**

```
输入: [ [1,2], [1,2], [1,2] ]

输出: 2

解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。
```

**示例 3:**

```
输入: [ [1,2], [2,3] ]

输出: 0

解释: 你不需要移除任何区间，因为它们已经是无重叠的了。
```

## 2、解题思路

​	这道题的关键就是利用贪心法，每一次，将跨越区域最多的删除，然后更新其他区间跨越的区间数量，然后继续更新

关键点在于，如何有效率的找出相互区间之前的重叠

- 首先，我们按照区间左边界进行排序
- 我们判断当前区间和前面的区间关系，如果当前区间的左边界，小于前面区间的右边界，肯定是要删除一个，消除重叠，这时候，我们判断，这两个区间，那个区间的右边界比较小，我们挑选小的按个，删除另一个，因为右边界更小，表示向后重叠的机会就越小



```python
# Definition for an interval.
# class Interval:
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e

class Solution:
    def eraseOverlapIntervals(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: int
        """
        intervals = sorted(intervals,key=lambda x: x.start)
        count = 0
        pre = 0
        for i in range(1,len(intervals)):
            if intervals[pre].end > intervals[i].start:
                pre = i if intervals[pre].end > intervals[i].end else pre
                count += 1
            else:
                pre = i
        return count
```

