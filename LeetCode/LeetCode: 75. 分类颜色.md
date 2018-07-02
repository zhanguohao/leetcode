# LeetCode: 75. 分类颜色

[TOC]

## 1、题目描述





给定一个包含红色、白色和蓝色，一共 *n* 个元素的数组，**原地**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

**注意:**
不能使用代码库中的排序函数来解决这道题。

**示例:**

```
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

**进阶：**

- 一个直观的解决方案是使用计数排序的两趟扫描算法。
  首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
- 你能想出一个仅使用常数空间的一趟扫描算法吗？





## 2、解题思路

​	如果是常规的排序，是很简单，但是不能一谈扫描就将数组排序好，但是这个数字有点特殊，他只有3中，0，1，2，也就是说，最后的结果是，0，1，2的顺序

​	因此，我们设置两个指针，分别指向排好序的0的右端，2的左端，然后不断地判断，如果当前数字是0，直接与左端的指针位置数交换，并且，左指针加一

​	如果是2，就与右面的2的指针位置交换，并且右指针减一

​	等到判断到右指针的时候，表示整个数组都排好序了

举个例子

```
[2,0,2,1,1,0]
```

左指针=0

右指针=5

然后从0号位置开始判断

判断是2，与右指针指向的位置交换，也就是变成了

```
[0,0,2,1,1,2]
```

右指针减一

right = 4

此时，

然后判断一下，发现现在指向的是0，但是当前指针与左指针相等，直接加一，左指针加一

index=1

left=1

发现是0，当前指针和左指针相等，直接加一，左指针加一

index=2

left=2

发现现在这个是2，与右指针交换，得到

```
[0,0,1,1,2,2]
```

然后index =3

right = 3

index = 4

然后继续，index超过了right，退出



```python
class Solution:
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        length = len(nums)
        if length <= 1:
            return

        left = 0
        right = length - 1
        index = 0
        while index <= right:
            if nums[index] == 0:
                if index == left:
                    index += 1
                    left += 1
                else:
                    nums[index], nums[left] = nums[left], nums[index]
                    left += 1

            elif nums[index] == 2:
                if index == right:
                    index += 1
                    right -= 1
                else:
                    nums[index], nums[right] = nums[right], nums[index]
                    right -= 1
            else:
                index += 1
```



