# LeetCode: 31. 下一个排列

[TOC]



## 1、题目描述





实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须**原地**修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
`1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1`





## 2、解题思路

​	实际上这个题目是有规律的，其想要求得就是全排列的时候，按照字典序输出

​	

例如，123的全排列，按照字典序

123

132

213

231

312

321



我们发现，实际上，规律是，从右面向左寻找，找出第一个非升序的数字，例如，123，第一个不是升序的，就是2，直接判断3，2，发现2比3小，是降序的，于是，就将2与后面的比他大的那个数交换，得到132



如果是

1243，该如何寻找呢

首先，判断，3，4是升序，然后，2，4降序，找到了2，于是，将2与3进行交换，然后将2后面的数重新排列，得到1324，这就是下一个排列



1. 从右向前寻找，第一个非升序的数字，然后将这个数字与后面第一个大于他的数字交换
2. 然后将这个数字后面的数字重新按照顺序排列即可



```python
class Solution:
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        if len(nums) <= 1:
            return
        index = 0
        for i in range(len(nums) - 1, 0, -1):
            if nums[i] > nums[i - 1]:
                index = i - 1
                break

        swap_index = len(nums) - 1

        # 寻找第一个比他大的数
        for i in range(len(nums) - 1, index, -1):
            if nums[i] > nums[index]:
                swap_index = i
                break

        nums[index], nums[swap_index] = nums[swap_index], nums[index]

        nums[index + 1:] = sorted(nums[index + 1:])
```





