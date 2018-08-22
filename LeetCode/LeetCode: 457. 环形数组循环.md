# LeetCode: 457. 环形数组循环

[TOC]

## 1、题目描述

给定一组含有正整数和负整数的数组。如果某个索引中的 n 是正数的，则向前移动 n 个索引。相反，如果是负数(-n)，则向后移动 n 个索引。

假设数组首尾相接。判断数组中是否有环。环中至少包含 2 个元素。环中的元素一律“向前”或者一律“向后”。

**示例 1：**给定数组 [2, -1, 1, 2, 2], 有一个循环，从索引 0 -> 2 -> 3 -> 0。

**示例 2：**给定数组[-1, 2], 没有循环。

**注意：**给定数组保证不包含元素"0"。

你能写出时间复杂度为 **O(n)** 且空间复杂度为 **O(1)** 的算法吗？

## 2、解题思路

    一开始没有理解题意，实际上，这道题目描述不够清楚

    基本题意如下：

- 数组的下标，对应一个偏移量，表示下一步能够到达的下标

  - ```
    举个例子
    输入：
    [2, -1, 1, 2, 2]
    我们将每一个下标，都计算出下一步的位置：
    [2, 0, 3, 0, 2]
    于是，我们发现
    0位置的下一步是2，然后到达位置2，下一步为3，接着到达的位置是0，形成了循环
    ```

- 需要注意的是，循环的形成，需要判断的是同一个方向形成环

  - ```
    例如
    [1,-1]
    转换后，
    [1，0]
    0->1->0，看似是这样，但实际上题目要求并不是这样的，这样是不成立的，需要是同一个方向
    ```



	这导体的解题思路与之前链表的回环检测异曲同工，同样是使用快慢指针，不过需要注意结束条件，在链表中，结束条件是找到了None，而在这里，结束条件就是前进的方向不同了

因此，解题思路就有了：

- 如果当前值为0，跳过
- 设置两个快慢指针，快指针一次前进两步，如果快指针的两步中遇到了调转方向，则结束循环
  - 判断快慢指针是否相同
    - 相同的话，当前值是不是0，不是0则返回True
- 将上一次的循环，也就是前一次走过的路径进行置零，判断过的路径不需要判断



```python
class Solution(object):
    def circularArrayLoop(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        
        
        length = len(nums)

        for i in range(length):

            if not nums[i]:
                continue

            slow = i
            fast = self.get_index(slow, nums)
            while nums[i] * nums[fast] > 0 and nums[i] * nums[self.get_index(fast, nums)] > 0:
                if slow == fast:
                    if slow == self.get_index(slow, nums):
                        break
                    return True
                slow = self.get_index(slow, nums)
                fast = self.get_index(self.get_index(fast, nums), nums)

            slow = i
            val = nums[i]
            while nums[slow] * val > 0:
                nums[slow] = 0
                slow = self.get_index(slow, nums)
        return False

    @staticmethod
    def get_index(index, nums):
        length = len(nums)
        return (index + nums[index] + length) % length
```

