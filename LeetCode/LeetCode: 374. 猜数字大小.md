# LeetCode: 374. 猜数字大小

[TOC]

## 1、题目描述



我们正在玩一个猜数字游戏。 游戏规则如下：
我从 **1** 到 **\*n*** 选择一个数字。 你需要猜我选择了哪个数字。
每次你猜错了，我会告诉你这个数字是大了还是小了。
你调用一个预先定义好的接口 `guess(int num)`，它会返回 3 个可能的结果（`-1`，`1` 或 `0`）：

```
-1 : 我的数字比较小
 1 : 我的数字比较大
 0 : 恭喜！你猜对了！
```

**示例:**

```
n = 10, 我选择 6.

返回 6.
```





## 2、解题思路



```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
# def guess(num):

class Solution(object):
    def guessNumber(self, n):
        """
        :type n: int
        :rtype: int
        """
        left = 0
        right = n

        while left < right - 1:
            if guess(right - (right - left) // 2) == 1:
                left = left + (right - left) // 2
            elif guess(right - (right - left) // 2) == -1:
                right = right - (right - left) // 2
            elif guess(right - (right - left) // 2) == 0:
                return right - (right - left) // 2

        if guess(left) == 0:
            return left
        else:
            return right
```





```c
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
# def guess(num):

class Solution(object):
    def guessNumber(self, n):
        """
        :type n: int
        :rtype: int
        """
        
        left = 1
        right = n
        mid = (left + right) // 2
        while guess(mid) != 0:
            if guess(mid) == 1:
                left = mid + 1
            else:
                right = mid - 1
            mid = (left + right) // 2

        return mid
        
```

