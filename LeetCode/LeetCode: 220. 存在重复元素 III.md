# LeetCode: 220. 存在重复元素 III

[TOC]



## 1、题目描述

​	

给定一个整数数组，判断数组中是否有两个不同的索引 *i* 和 *j*，使得 **nums [i]** 和 **nums [j]** 的差的绝对值最大为 *t*，并且 *i* 和 *j* 之间的差的绝对值最大为 *ķ*。

**示例 1:**

```
输入: nums = [1,2,3,1], k = 3, t = 0
输出: true
```

**示例 2:**

```
输入: nums = [1,0,1,1], k = 1, t = 2
输出: true
```

**示例 3:**

```
输入: nums = [1,5,9,1,5,9], k = 2, t = 3
输出: false
```



## 2、解题思路

​	从题意中看，实际上，我们能够看到，也就是说，假设有一个窗口，能够向前滑动的话，如果当前窗口的元素都判断完毕，那么，窗口向前滑动一个元素，抽口中的所有元素都与最后一个元素进行差运算，如果过满足条件，即可返回True

​	需要注意的是，第一个窗口较为特殊，每个元素都要与其他的元素进行判断

​	这样，等到窗口滑动到最后的时候，我们就能直接得出结果了

​	超出时间限制了。。。！

```python
class Solution:
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        """
        :type nums: List[int]
        :type k: int
        :type t: int
        :rtype: bool
        """
        length = len(nums)
        if length <= k + 1:
            for i in range(length):
                for j in range(i + 1, length):
                    if abs(nums[i] - nums[j]) <= t:
                        return True
        else:
            for i in range(k + 1):
                for j in range(i + 1, k+1):
                    if abs(nums[i] - nums[j]) <= t:
                        return True

            for i in range(k + 1, length):
                for j in range(1, k + 1):
                    if abs(nums[i] - nums[i - j]) <= t:
                        return True
        return False
```

​	算法没问题，不过既然超出了时间限制，那么就需要换种思路

​	

​	换一种思路，使用桶的思路

​	我们找出最小值，将当前值减去最小值，然后除以t+1, 得到t+1个桶，于是，只需要判断当前桶，前后桶即可

​	这个桶，可以使用哈希实现

​	下面这个通过了。

```python
class Solution:
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        """
        :type nums: List[int]
        :type k: int
        :type t: int
        :rtype: bool
        """
        
        length = len(nums)
        if length <= 1 or t < 0 or k < 1:
            return False

        min_value = min(nums)

        buff = {}
        for i in range(length):
            if i > k:
                key = (nums[i - k - 1] - min_value) // (t + 1)
                buff.pop(key)

            key = (nums[i] - min_value) // (t + 1)
            if buff.get(key) is not None:
                return True

            small = buff.get(key - 1)
            if small is not None and (nums[i] - small) <= t:
                return True
            big = buff.get(key + 1)
            if big is not None and (big - nums[i]) <= t:
                return True

            buff[key] = nums[i]

        return False
```



​	







