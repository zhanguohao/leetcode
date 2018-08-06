# LeetCode: 392. 判断子序列

[TOC]



## 1、题目描述

给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。

你可以认为 **s** 和 **t** 中仅包含英文小写字母。字符串 **t** 可能会很长（长度 ~= 500,000），而 **s** 是个短字符串（长度 <=100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

**示例 1:**
**s** = `"abc"`, **t** = `"ahbgdc"`

返回 `true`.

**示例 2:**
**s** = `"axc"`, **t** = `"ahbgdc"`

返回 `false`.

**后续挑战** **:**

如果有大量输入的 S，称作S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

**致谢:**

特别感谢 [@pbrother ](https://leetcode.com/pbrother/)添加此问题并且创建所有测试用例。



## 2、解题思路

​	为啥感觉这道题直接用双指针就可以了？好像没get到难点。。

```python
class Solution:
    def isSubsequence(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        index_s = 0
        index_t = 0

        length_s = len(s)
        length_t = len(t)
        
        if length_s == 0:
            return True
        

        while index_t < length_t:

            if s[index_s] == t[index_t]:
                index_s += 1
            if index_s >= length_s:
                return True
            index_t += 1

        return False
```

