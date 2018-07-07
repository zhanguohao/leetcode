# LeetCode: 260. 只出现一次的数字 III

[TOC]



## 1、题目描述



给定一个整数数组 `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。

**示例 :**

```
输入: [1,2,1,3,2,5]
输出: [3,5]
```

**注意：**

1. 结果输出的顺序并不重要，对于上面的例子， `[5, 3]` 也是正确答案。
2. 你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？



## 2、解题思路

​	看到这道题，结合之前题目的思路，如果通过异或，我们就能够得到只出现一次的两个数的异或值

​	然后找到其中的一位为1的，也就是说这一位，两个数字不同，我们根据这一位，将所有的数据分成两组，然后进行异或，得到了两个数，就是我们想要的结果

​	

```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        xor_nums = 0
        for i in nums:
            xor_nums ^= i

        xor_nums &= -xor_nums

        num1 = 0
        num2 = 0
        for i in nums:
            if xor_nums & i:
                num1 ^= i
            else:
                num2 ^= i

        return [num1, num2]
```

