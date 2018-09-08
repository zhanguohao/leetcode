# LeetCode: 896. 单调数列

[TOC]

## 1、题目描述

如果数组是单调递增或单调递减的，那么它是*单调的*。

如果对于所有 `i <= j`，`A[i] <= A[j]`，那么数组 `A` 是单调递增的。 如果对于所有 `i <= j`，`A[i]> = A[j]`，那么数组 `A` 是单调递减的。

当给定的数组 `A` 是单调数组时返回 `true`，否则返回 `false`。



**示例 1：**

```
输入：[1,2,2,3]
输出：true
```

**示例 2：**

```
输入：[6,5,4,4]
输出：true
```

**示例 3：**

```
输入：[1,3,2]
输出：false
```

**示例 4：**

```
输入：[1,2,4,5]
输出：true
```

**示例 5：**

```
输入：[1,1,1]
输出：true
```

 

**提示：**

1. `1 <= A.length <= 50000`
2. `-100000 <= A[i] <= 100000`

## 2、解题思路

	题目很简单，直接判断第一个元素和最后一个元素比较大小，然后遍历一遍，如果单调性相同，返回True，遇到不同的，返回False



```python
class Solution:
    def isMonotonic(self, A):
        """
        :type A: List[int]
        :rtype: bool
        """

        '''
        direct:
            -1: 单调递减
            1： 单调递增
        '''
        direct = -1

        length = len(A)
        if length <= 1:
            return True

        if A[-1] >= A[0]:
            direct = 1

        for i in range(length - 1):

            if A[i] > A[i + 1] and direct > 0:
                return False

            if A[i] < A[i + 1] and direct < 0:
                return False
        return True
```

	