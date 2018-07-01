# LeetCode: 532. 数组中的K-diff数对

[TOC]

## 1、题目描述



给定一个整数数组和一个整数 **k**, 你需要在数组里找到**不同的** k-diff 数对。这里将 **k-diff** 数对定义为一个整数对 (i, j), 其中 **i** 和 **j** 都是数组中的数字，且两数之差的绝对值是 **k**.

**示例 1:**

```
输入: [3, 1, 4, 1, 5], k = 2
输出: 2
解释: 数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
尽管数组中有两个1，但我们只应返回不同的数对的数量。
```

**示例 2:**

```
输入:[1, 2, 3, 4, 5], k = 1
输出: 4
解释: 数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5)。
```

**示例 3:**

```
输入: [1, 3, 1, 5, 4], k = 0
输出: 1
解释: 数组中只有一个 0-diff 数对，(1, 1)。
```

**注意:**

1. 数对 (i, j) 和数对 (j, i) 被算作同一数对。
2. 数组的长度不超过10,000。
3. 所有输入的整数的范围在 [-1e7, 1e7]。



## 2、解题思路

### 2.1 哈希表法

​	一种思路是使用哈希表，将每个数字作为键，出现的次数作为value

​	判断k是不是0，如果是，那么将所有的value值大于2的统计出来

​	如果k大于0，依次判断每一个键加上k是不是在哈希表中

​	

```python
class Solution:
    def findPairs(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        result = 0;
        
        if k<0:
            return 0
        
        hash_nums = {}
        for i in nums:
            if (hash_nums.get(i) == None):
                hash_nums[i] = 1
            else:
                hash_nums[i] += 1

        for i in hash_nums.keys():
            if k==0 :
                if hash_nums[i] >=2:
                    result += 1
            else:
                if hash_nums.get(i+k) != None:
                    result += 1

        return result
```

​	用python实现比较简单



### 2.2 排序法

​	将数组先排序

​	判断k是不是0，如果是，就不断寻找相同数字大于2的，找到一次，就加一

