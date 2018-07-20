# LeetCode: 862. 和至少为 K 的最短子数组

[TOC]



## 1、题目描述

​	返回 `A` 的最短的非空连续子数组的**长度**，该子数组的和至少为 `K` 。

如果没有和至少为 `K` 的非空子数组，返回 `-1` 。

 


**示例 1：**

```
输入：A = [1], K = 1
输出：1
```

**示例 2：**

```
输入：A = [1,2], K = 4
输出：-1
```

**示例 3：**

```
输入：A = [2,-1,2], K = 3
输出：3
```

 

**提示：**

1. `1 <= A.length <= 50000`
2. `-10 ^ 5 <= A[i] <= 10 ^ 5`
3. `1 <= K <= 10 ^ 9`



## 2、解题思路

​	一开始，直接想到暴力破解法，直接算出从位置0到当前位置的和，然后每个位置遍历一遍，减去从位置0一直到当前位置的差值，如果能够满足条件，更新结果

​	这种暴力破解法，超出了时间限制

​	后来又想通过动态规划来做，以前做过一道题，叫做最大子序列和，也就是找出子序列中，最大的连续的和是多少，不过这道题反其道而行之，找出最小的满足条件的值，

​	

​	换个思路，使用滑动窗口的办法，首先，设置两个指针，一个纸箱滑动窗口左面，一个纸箱右面，还有一个和，表示当前窗口的和

- 如果和小于K，就将r右移
- 如果和小于0，将左指针和右指针一块移动，重新开始
- 如果当前和大于k，左指针右移
- 每一次遇到和大于K的情况，都更新result



​	在写代码的时候，遇到一个问题，就是如果在左面的指针向右移动的时候，如果说已经判断当前窗口的值小于K，就继续移动right，实际上这样不行，因为前面可能还有负数，也就是说，left并没有移动到对应的位置上去

​	所以，每一次找到的时候，我们需要将这个子数组前面所有的都尝试减去一遍，得到满足的值就更新result，并且，还需要将left放到对应的位置上去

​	我们设置两个指针，一个是在这个过程中，left向右移动，最后一个满足和的要求的left，另一个则是在right处向左寻找，找到不为0的那个值，直到left结束

​	



​	这种思路，与求解最大子序列和有相似的地方，也有一点不同，最大子序列和并不需要移动左指针，只需要在和小于0的时候移动，

​	所以，从这里可以看出来，凡是寻找连续子数组的情况，都能够用滑动窗口来解决

​	哎，没想到，滑动窗口也超过时间限制了，再换一种



### 2.1 暴力破解法

```python
import itertools


class Solution:
    def shortestSubarray(self, A, K):
        """
        :type A: List[int]
        :type K: int
        :rtype: int
        """
        order_sums = list(itertools.accumulate(A))

        print(order_sums)

        result = 100000

        for i in range(len(A)):
            if result <= 1:
                break
            if order_sums[i] >= K:
                result = min(result, i + 1)
            for j in range(i):
                if order_sums[i] - order_sums[j] >= K and i - j < result:
                    result = i - j
        if result == 100000:
            return -1
        else:
            return result


a = Solution()

print(a.shortestSubarray([77, 19, 35, 10, -14], 19))

```

​	暴力破解法超过了时间限制



### 2.2 滑动窗口法

​	

```python
class Solution:
    def shortestSubarray(self, A, K):
        """
        :type A: List[int]
        :type K: int
        :rtype: int
        """

        left = 0
        right = -1
        windows_sum = 0

        length = len(A)

        result = length + 1

        max_value = -100000000
        temp_sum = 0
        temp_left = 0
        while left < length:

            if right + 1 < length and windows_sum < K:
                right += 1
                windows_sum += A[right]

            else:
                windows_sum -= A[left]
                left += 1

                temp_sum = 0
                max_value = -100000000
                temp_left = left
                for i in range(right, left - 1, -1):
                    temp_sum += A[i]
                    if temp_sum > max_value:
                        max_value = temp_sum
                        temp_left = i

                windows_sum = max_value
                left = temp_left

            if windows_sum >= K:
                result = min(result, right - left + 1)
            elif windows_sum < 0:
                left = right + 1
                windows_sum = 0

        if result == length + 1:
            return -1
        return result
```



​	哎，这个方法也超过时间限制了



### 2.3 滑动窗口改进 

​	实际上，前面的方法还是有点复杂，实际上，同样是滑动窗口，并不一定非得要在原来的数组上滑动，也可以在我们构造的累加和数组上滑动

​	例如，构造一个数组，如果right指向的数减去left指向的数能够大于等于K，更新结果值

​	借助双端队列，我们在队列中，保存前面没有判断过的，并且比当前的累积和小的累加和，进行判断，判断完毕以后，就将最小的那个弹出，因为用不到了，还需要向后判断，距离会变大，所以最左端的就用不到了

​	注意，双端队列里面保存的是索引值，这样才计算结果值

​	构造和数列的时候，在前面放置一个0，这样我们才能得到与第一个元素的差值

例如[1,2,3]

和数列

[1,3,6]

​	如果我们不放置一个0在前面，就会发现我们用3-1，6-1，得到的是第2个元素，第2，3元素之和，无法得到第0元素之和

​	所以，放置一个0才可以



​	这一次时间没有超过限制

```python
import collections

class Solution:
    def shortestSubarray(self, A, K):
        """
        :type A: List[int]
        :type K: int
        :rtype: int
        """
        
        length = len(A)
        result = length + 1
        if length <= 0:
            return 0
        sums = [0] + list(itertools.accumulate(A))
        less_than = collections.deque()
        
        for index, cur_sum in enumerate(sums):

            while less_than and cur_sum <= sums[less_than[-1]]:
                less_than.pop()

            while less_than and cur_sum - sums[less_than[0]] >= K:
                result = min(result, index - less_than[0])
                less_than.popleft()

            less_than.append(index)
            
        if result == length + 1:
            return -1
        else:
            return result
```

​
