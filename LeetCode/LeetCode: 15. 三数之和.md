# LeetCode: 15. 三数之和

[TOC]



## 1、题目描述



给定一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

```
例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```





## 2、解题思路



​	首先是统计数量，分成两种情况讨论，一种是结果中有重复值，另一种没有

​	然后进行排序，固定左面的值，在右面的值中，寻找两数之和为当前值的负数，这个可以利用leetcode的第一题的思路来寻找

​	

```python
class Solution:
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if len(nums) < 3:
            return []

        result = []

        t = collections.Counter(nums)
        # count_big_two = [v for v in t if t[v] >= 2]
        count_equal_one = sorted(list(t))

        if t[0] >= 3:
            result.append([0, 0, 0])

        for i in range(len(count_equal_one)):
            if count_equal_one[i] >= 0:
                break
          
            if count_equal_one[i] != 0 and t[count_equal_one[i]] >= 2 and t[-count_equal_one[i] * 2] > 0:
                result.append([count_equal_one[i], count_equal_one[i], -2 * count_equal_one[i]])

            if count_equal_one[i] != 0 and count_equal_one[i] % 2 == 0 and t[-count_equal_one[i] // 2] >= 2:
                result.append([count_equal_one[i], -count_equal_one[i] // 2, -count_equal_one[i] // 2])

            temp = {}
            for j in range(i + 1, len(count_equal_one)):
                if count_equal_one[j] in temp:
                    result.append([count_equal_one[i], count_equal_one[j], temp[count_equal_one[j]]])
                else:
                    temp[-count_equal_one[i] - count_equal_one[j]] = count_equal_one[j]

        return result
        
```

