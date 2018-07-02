# LeetCode: 74. 搜索二维矩阵

[TOC]



## 1、题目描述



编写一个高效的算法来判断 *m* x *n* 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

- 每行中的整数从左到右按升序排列。
- 每行的第一个整数大于前一行的最后一个整数。

**示例 1:**

```
输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
输出: true
```

**示例 2:**

```
输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
输出: false
```



## 2、解题思路

​	从题意来看，实际上这个矩阵每一行遍历得到的就是排序好的数组，直接用二分法就行了，只需要将左右指针转化为二维坐标即可

​	没有任何难点，只要注意一下边界条件即可，空的矩阵返回FALSE



```python
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        
        
        row = len(matrix)
        if row <=0:
            return False
        col = len(matrix[0])
        left = 0
        right = row * col - 1

        mid = (left + right) // 2

        while left <= right:
            if matrix[mid // col][mid % col] == target:
                return True
            else:
                if matrix[mid // col][mid % col] > target:
                    right = mid - 1
                else:
                    left = mid + 1
            mid = (left + right) // 2

        return False
```

