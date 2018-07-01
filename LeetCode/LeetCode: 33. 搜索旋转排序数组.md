# LeetCode: 33. 搜索旋转排序数组

[TOC]



## 1、题目描述



假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 *O*(log *n*) 级别。

**示例 1:**

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```

**示例 2:**

```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```







## 2、解题思路



​	同样是使用二分法，不过在更新左右范围的时候有所不同

​	如果中间值大于最左面的那个值，表示中间值目前位与左面递增序列中，这时候，我们判断一下中间值是不是大于target，并且左面的值小于等于target，这样就转化了，变成在左面的范围中target

​	如果中间值小于左面的值，中间值肯定是在右面的递增序列上面，同样的道理，这时候，我们就需要判断target是不是在右面的这段递增序列上，然后更新left，right



```python
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        result_index = -1

        left = 0
        right = len(nums) - 1

        middle = (left + right) // 2

        while left <= right:

            if nums[middle] == target:
                result_index = middle
                break

            # 如果middle 位与左面的递增数组上面
            if nums[middle] >= nums[left]:
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

        return result_index
```



