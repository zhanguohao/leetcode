# LeetCode: 395. 至少有K个重复字符的最长子串

[TOC]

## 1、题目描述



找到给定字符串（由小写字符组成）中的最长子串 **T** ， 要求 **T** 中的每一字符出现次数都不少于 *k* 。输出 **T** 的长度。

**示例 1:**

```
输入:
s = "aaabb", k = 3

输出:
3

最长子串为 "aaa" ，其中 'a' 重复了 3 次。
```

**示例 2:**

```
输入:
s = "ababbc", k = 2

输出:
5

最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。
```



## 2、解题思路

​	使用分治法

- 首先，统计当前字符串所有字符出现的次数
- 然后遍历字符串
- 如果当前字符出现的次数小于k，那么判断下一个字符出现的次数是不是小于k，如果是大于k的，就使用当前位置进行分割，分割为两个字符串，回到第一步，重新开始判断

```python
class Solution:
    def longestSubstring(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        if len(s) < k:
            return 0

        count = collections.Counter(s)

        index = 0
        while index < len(s):
            if count[s[index]] < k:
                first_end = index
                while index < len(s) and count[s[index]] < k:
                    index += 1
                return max(self.longestSubstring(s[0:first_end], k), self.longestSubstring(s[index:], k))
            index += 1
        return len(s)
```



