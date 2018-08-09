# LeetCode: 398. 随机数索引

[TOC]

## 1、题目描述

给定一个可能含有重复元素的整数数组，要求随机输出给定的数字的索引。 您可以假设给定的数字一定存在于数组中。

**注意：**
数组大小可能非常大。 使用太多额外空间的解决方案将不会通过测试。

**示例:**

```c++
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) 应该返回索引 2,3 或者 4。每个索引的返回概率应该相等。
solution.pick(3);

// pick(1) 应该返回 0。因为只有nums[0]等于1。
solution.pick(1);
```

## 2、解题思路

​	这道题目与382题，链表随机节点很相似，使用蓄水池抽样即可，蓄水池抽样可以参见对应的笔记；

- 首先，我们判断这个元素在数组中出现的总的次数，并且找出第一次出现的位置
- 如果恰好出现1次，直接返回结果
- 如果是多次，使用蓄水池抽样，找出其中一个索引即可

```python
class Solution:
    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.nums = nums

    def pick(self, target):
        """
        :type target: int
        :rtype: int
        """
        count = self.nums.count(target)
        cur_pos = self.nums.index(target)
        res = cur_pos
        if count == 1:
            return res

        for i in range(1, count):
            cur_pos = self.nums[cur_pos + 1:].index(target) + cur_pos + 1
            if random.randint(0, i) == 0:
                res = cur_pos
        return res

# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.pick(target)
```

