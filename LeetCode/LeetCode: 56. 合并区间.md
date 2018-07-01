# LeetCode: 56. 合并区间

[TOC]



## 1、题目描述

​	



给出一个区间的集合，请合并所有重叠的区间。

**示例 1:**

```
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

**示例 2:**

```
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```





## 2、解题思路

​		这个问题实际上很简单，只要排个序，然后从前向后合并就好了

- 如果当前的end大于等于后面一个元素的start，这时候就需要合并，如果不是，直接将current加入到result里面，然后current指向下一个
- 如果已经合并到了最后一个，直接将current放到结果中，返回result



```python
# Definition for an interval.
# class Interval:
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e

class Solution:
    def merge(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: List[Interval]
        """
        length = len(intervals)
        if length <=0:
            return []
        temp = sorted(intervals, key=lambda x: x.start)
        result = []

        current = temp[0]
        for i in range(length):
            if i + 1 < length:
                if temp[i + 1].start <= current.end:
                    current.end = max(current.end, temp[i + 1].end)
                else:
                    result.append(current)
                    current = temp[i + 1]
            else:
                result.append(current)
        return result
```

