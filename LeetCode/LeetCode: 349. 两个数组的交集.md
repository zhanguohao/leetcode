# LeetCode: 349. 两个数组的交集

[TOC]



## 1、题目描述



给定两个数组，写一个函数来计算它们的交集。

**例子:**

 给定 *num1*= `[1, 2, 2, 1]`, *nums2* = `[2, 2]`, 返回 `[2]`.

**提示:**

- 每个在结果中的元素必定是唯一的。
- 我们可以不考虑输出结果的顺序。



## 2、解题思路

​	直接使用python的set解题很简单，如下：

```python
class Solution:
    def intersection(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        s1 = set(nums1)
        s2 = set(nums2)
        return list(s1 & s2)
```

