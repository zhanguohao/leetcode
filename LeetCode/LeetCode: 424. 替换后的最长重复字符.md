# LeetCode: 424. 替换后的最长重复字符

[TOC]

## 1、题目描述

给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 *k* 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

**注意:**
字符串长度 和 *k* 不会超过 104。

**示例 1:**

```
输入:
s = "ABAB", k = 2

输出:
4

解释:
用两个'A'替换为两个'B',反之亦然。
```

**示例 2:**

```
输入:
s = "AABABBA", k = 1

输出:
4

解释:
将中间的一个'A'替换为'B',字符串变为 "AABBBBA"。
子串 "BBBB" 有最长重复字母, 答案为 4。
```

## 2、解题思路

​	这个问题可以使用滑动窗口来做

- 首先判断length<=k+1? 是的话，直接返回length，不是的话，继续判断
- 初始的窗口大小设置为k+1，这样的话，肯定能替换出来
- 然后统计一下窗口内的字符的数量，找出最大的那个数量m
- 判断，如果窗口大小-m<=k,窗口右指针向右移动
- 同样继续判断，窗口大小-m>窗口左指针向右移动



```python
class Solution:
    def characterReplacement(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        length = len(s)

        if length <= k + 1:
            return length

        ans = k + 1
        start = 0
        end = k + 1
        count = collections.Counter(s[start:end])

        while start < length:
            most = count.most_common(1)[0][1]
            if end - start - most <= k:
                ans = max(ans, end - start)
                if end < length:
                    count[s[end]] += 1
                    end += 1
                    continue
            count[s[start]] -= 1
            start += 1

        return ans
```

