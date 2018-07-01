# LeetCode: 16. 最接近的三数之和

[TOC]

## 1、题目描述





给定一个包括 *n* 个整数的数组 `nums` 和 一个目标值 `target`。找出 `nums` 中的三个整数，使得它们的和与 `target` 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
```





## 2、解题思路

​	这道题与第15题有相似之处，找到3个数之和，使得他们的和最接近给定的数

​	假如我们将整个数组排序了，每次固定第一个数，在后面进行查找

​	如果diff变小，更新结果值

​	3数之和小于target，left增加

​	3数之和大于target，right减一，表示需要减小



```c
class Solution:
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        sorted_num = sorted(nums)

        diff = 0xffffffff

        result = 0

        for i in range(len(sorted_num)):

            left = i + 1
            right = len(sorted_num) - 1
            while left < right:
                temp_sum = sorted_num[i] + sorted_num[left] + sorted_num[right]
                temp_diff = abs(target - temp_sum)

                if temp_diff == 0:
                    return temp_sum

                if temp_diff < diff:
                    diff = temp_diff
                    result = temp_sum

                if temp_sum < target:
                    left += 1
                else:
                    right -= 1
        return result
```

