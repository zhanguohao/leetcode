# LeetCode: 215. 数组中的第K个最大元素

[TOC]

## 1、题目描述



在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

**示例 1:**

```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

**说明:** 

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。



## 2、解题思路

​	一般直接想到的就是排序，然后找出来，但是快排的的话，时间复杂度也是$O(nlog(n))$的，这个时间复杂度，有堆排序，快排，还有归并排序，考虑不同的排序的特点，我们发现，堆排序在排序的过程中，就能找出第K个元素

​	堆是完全二叉树，	堆排序的基本思路是建立一个最大堆，然后将最前面的元素，也就是最大的元素与最后一个元素交换位置，然后将前面的元素调整成堆，不断地循环，最后得到排序数组

​	调整成堆在于将后面的每一个子树都调整成堆，然后整棵树就变成了堆

```python
class Solution:
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        size = len(nums)
        self.build_heap(nums, size)
        cur = size - 1
        for i in range(k-1):
            nums[0], nums[cur] = nums[cur], nums[0]
            self.heapify(nums, 0, cur)
            cur -= 1

        return nums[0]

    def build_heap(self, nums, size):
        heap_size = (size - 1) // 2
        while heap_size >= 0:
            self.heapify(nums, heap_size, size)
            heap_size -= 1

    def heapify(self, nums, node, size):
        left_child = 2 * node + 1
        right_child = 2 * node + 2

        max_node = node

        if left_child < size and nums[max_node] < nums[left_child]:
            max_node = left_child

        if right_child < size and nums[max_node] < nums[right_child]:
            max_node = right_child

        if max_node != node:
            nums[node], nums[max_node] = nums[max_node], nums[node]
            self.heapify(nums, max_node, size)
```



​	写完以后，看了一下其他人的解答， 直接使用的库函数。。。。