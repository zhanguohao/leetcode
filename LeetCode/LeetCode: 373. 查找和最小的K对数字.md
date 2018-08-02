# LeetCode: 373. 查找和最小的K对数字

[TOC]



## 1、题目描述



给定两个以升序排列的整形数组 **nums1** 和 **nums2**, 以及一个整数 **k**。

定义一对值 **(u,v)**，其中第一个元素来自 **nums1**，第二个元素来自 **nums2**。

找到和最小的 k 对数字 **(u1,v1), (u2,v2) ... (uk,vk)**。

**示例 1:**

```
给出： nums1 = [1,7,11], nums2 = [2,4,6],  k = 3

返回： [1,2],[1,4],[1,6]

返回序列中的前 3 对数：
[1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

**示例 2:**

```
给出：nums1 = [1,1,2], nums2 = [1,2,3],  k = 2

返回： [1,1],[1,1]

返回序列中的前 2 对数：
[1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

**示例 3:**

```
给出：nums1 = [1,2], nums2 = [3],  k = 3 

返回： [1,3],[2,3]

也可能序列中所有的数对都被返回:
[1,3],[2,3]
```



## 2、解题思路

​	借助堆来实现

- 首先用用第一个数组的第0个元素，加上第二个数组的所有的值，放入堆中
- 然后开始从堆里里面取值，如果取出的值大于第一个数字第1个元素加上第二个数组第0个元素的值，这时候，我们将第一个数组的第1各元素，与第二个数组的所有元素匹配，放入堆中



实例说明

```
[1 1 2] [1 2 3]
c = 0
一开始，将第一个元数组的第0个元素，与第二个数组所有的数字匹配，放入堆中
(2，0，0)
(3，0，1)
(4，0，2)
c=1
将上面的组合放到堆中，接着，我们开始判断，从堆中取出来的组合，(2，0，0)的2，是不是大于第一个数组中下一个元素，与第二个数组中第0各元素之和，如果大于，结果中存放这个数字，并且将刚刚取出来的放入堆中，并且将第二个元素与剩余所有元素的匹配放入堆中

```

```python
class Solution:
    def kSmallestPairs(self, nums1, nums2, k):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :type k: int
        :rtype: List[List[int]]
        """
        
        if not nums1 or not nums2:
            return []

        length1 = len(nums1)
        length2 = len(nums2)

        count = min(length1 * length2, k)

        result = []

        heap = [(nums1[0] + nums2[i], 0, i) for i in range(length2)]
        heapq.heapify(heap)
        c = 1

        for i in range(count):
            if not heap:

                for j in range(length2):
                    heapq.heappush(heap, (nums1[c] + nums2[j], c, j))
                c += 1
            temp = heapq.heappop(heap)

            if c < length1:
                temp_sum = nums1[c] + nums2[0]
                if temp[0] < temp_sum:
                    result.append([nums1[temp[1]], nums2[temp[2]]])
                else:
                    result.append([nums1[c], nums2[0]])
                    heapq.heappush(heap, temp)
                    for j in range(1, length2):
                        heapq.heappush(heap, (nums1[c] + nums2[j], c, j))
                    c += 1
            else:
                result.append([nums1[temp[1]], nums2[temp[2]]])

        return result
```

