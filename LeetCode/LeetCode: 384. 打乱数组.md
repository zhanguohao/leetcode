# LeetCode: 384. 打乱数组

[TOC]

## 1、题目描述

打乱一个没有重复元素的数组。

**示例:**

```
// 以数字集合 1, 2 和 3 初始化数组。
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。
solution.shuffle();

// 重设数组到它的初始状态[1,2,3]。
solution.reset();

// 随机返回数组[1,2,3]打乱后的结果。
solution.shuffle();
```



## 2、解题思路

- 首先，我们保存原来的数组，在重置的时候，直接返回
- 然后，使用交换的思路，对每一个数，求解他想要取得位置，使用一个随机数，随机生成

​	当然，也可以直接使用random库中自带的shuffle函数

```python
class Solution:
    
    def __init__(self, nums):
        """
        :type nums: List[int]
        """

        self.origin = nums

    def reset(self):
        """
        Resets the array to its original configuration and return it.
        :rtype: List[int]
        """
        return self.origin

    def shuffle(self):
        """
        Returns a random shuffling of the array.
        :rtype: List[int]
        """
        cur = self.origin.copy()
        length = len(self.origin)
        for i in range(length):
            pos = random.randint(0, length - 1)
            cur[i], cur[pos] = cur[pos], cur[i]
        return cur
    
    
# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.reset()
# param_2 = obj.shuffle()
```



```python
class Solution:
    
    def __init__(self, nums):
        """
        :type nums: List[int]
        """

        self.origin = nums

    def reset(self):
        """
        Resets the array to its original configuration and return it.
        :rtype: List[int]
        """
        return self.origin

    def shuffle(self):
        """
        Returns a random shuffling of the array.
        :rtype: List[int]
        """
        cur = self.origin.copy()
        random.shuffle(cur)
        return cur
    
    
# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.reset()
# param_2 = obj.shuffle()
```

