# LeetCode: 594. 最长和谐子序列

[TOC]

## 1、题目描述





和谐数组是指一个数组里元素的最大值和最小值之间的差别正好是1。

现在，给定一个整数数组，你需要在所有可能的子序列中找到最长的和谐子序列的长度。

**示例 1:**

```
输入: [1,3,2,2,5,2,3,7]
输出: 5
原因: 最长的和谐数组是：[3,2,2,2,3].
```

**说明:** 输入的数组长度最大不超过20,000.





## 2、解题思路

​	看错了题意了，实际上是寻找相邻的两个数，加起来出现的次数最多！

​	直接用hashmap做



```python
class Solution:
    def findLHS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        hash_num = {}

        for i in nums:
            if hash_num.get(i) == None:
                hash_num[i] = 1
            else:
                hash_num[i] += 1

        max_value = 0
        for i in hash_num.keys():
            value1 = hash_num.get(i,-1)
            value2 = hash_num.get(i+1,-1)

            if value1 != -1 and value2 != -1:
                if max_value < value1 + value2:
                    max_value = value1 + value2

        return max_value
        
```



​	实际上，还可以使用collections里面的Counter来做

