# LeetCode: 347. 前K个高频元素

[TOC]



## 1、题目描述



给定一个非空的整数数组，返回其中出现频率前 **k** 高的元素。

例如，

给定数组 `[1,1,1,2,2,3]` , 和 k = 2，返回 `[1,2]`。

**注意：**

- 你可以假设给定的 *k* 总是合理的，1 ≤ k ≤ 数组中不相同的元素的个数。
- 你的算法的时间复杂度**必须**优于 O(*n* log *n*) , *n* 是数组的大小。



## 2、解题思路

​	先用字典，记录每个值出现的次数，然后通过排序，找出前面K个

```python
class Solution:
    def topKFrequent(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        counter = collections.Counter(nums)
        result = [v[0] for v in counter.most_common()[:k]]
        return result
```

​	直接用系统库，果然很省力