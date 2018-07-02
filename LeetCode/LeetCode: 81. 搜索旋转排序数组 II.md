# LeetCode: 81. 搜索旋转排序数组 II

[TOC]

## 1、题目描述

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,0,1,2,2,5,6]` 可能变为 `[2,5,6,0,0,1,2]` )。

编写一个函数来判断给定的目标值是否存在于数组中。若存在返回 `true`，否则返回 `false`。

**示例 1:**

```
输入: nums = [2,5,6,0,0,1,2], target = 0
输出: true
```

**示例 2:**

```
输入: nums = [2,5,6,0,0,1,2], target = 3
输出: false
```

**进阶:**

- 这是 [搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/description/) 的延伸题目，本题中的 `nums`  可能包含重复元素。
- 这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？



## 2、解题思路

​	前面33题，搜索旋转排序数组，之前的是没有重复元素的，实际上这道题的解题思路与前面没什么区别，需要注意的就是，要判断等于的情况

​	左右边界的问题，在前面，我们能够直接判断的情况，现在需要继续判断

[1,3,1,1,1] 

3

如上面的例子，如果出现了这种情况，我们需要知道要排除哪一边才行

我们分为4种情况，

middle=left=right

​	这种情况，需要判断是左面的相等，还是右面的相等，将相等的删掉

middle = left  middle≠right

​	直接删除左面的

middle ≠ left  middle=right

​	直接删除右面的

middle ≠ left  middle≠right

​	按照正常情况讨论

出现不同情况，分别进行讨论





实际上，只要判断中间值是不是等于两边中的一个就好了，如果中间值等于



主要是，每一次进行二分法的时候，要排除哪一边才好

不过呢，用python没这么麻烦，直接判断数量就好了

```python
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: bool
        """
        
        
        if nums.count(target) >= 1:
            return True
        return False
        
```

​	不过呢，这个办法有点取巧，而且效率不高



```python
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: bool
        """
        left = 0
        right = len(nums) - 1

        middle = (left + right) // 2

        while left <= right:

            if nums[middle] == target:
                return True

            equal = True
            if nums[middle] == nums[left] and nums[middle] == nums[right]:
                for i in range(left, middle):
                    if nums[i] != nums[left]:
                        equal = False
                        break
                if equal:
                    left = middle + 1
                else:
                    right = middle - 1
            elif nums[middle] == nums[left] and nums[middle] != nums[right]:
                left = middle + 1
            elif nums[middle] != nums[left] and nums[middle] == nums[right]:
                right = middle - 1
            else:

                if nums[middle] > nums[left]:
                    if nums[left] <= target and target < nums[middle]:
                        right = middle - 1
                    else:
                        left = middle + 1

                else:
                    # 现在middle位与右面的递增数组上面
                    if nums[right] >= target and target > nums[middle]:
                        left = middle + 1
                    else:
                        right = middle - 1

            middle = (left + right) // 2

        return False


```



