# LeetCode: 290. 单词模式

[TOC]

## 1、题目描述

给定一种 `pattern(模式)` 和一个字符串 `str` ，判断 `str` 是否遵循相同的模式。

这里的**遵循**指完全匹配，例如， `pattern` 里的每个字母和字符串 `str` 中的每个非空单词之间存在着双向连接的对应模式。

**示例1:**

```
输入: pattern = "abba", str = "dog cat cat dog"
输出: true
```

**示例 2:**

```
输入:pattern = "abba", str = "dog cat cat fish"
输出: false
```

**示例 3:**

```
输入: pattern = "aaaa", str = "dog cat cat dog"
输出: false
```

**示例 4:**

```
输入: pattern = "abba", str = "dog dog dog dog"
输出: false
```

**说明:**
你可以假设 `pattern` 只包含小写字母， `str` 包含了由单个空格分隔的小写字母。   



## 2、解题思路

​	在前面，205题中，做过类似的题目，不过当时是两个字符串进行比较，通过建立映射关系，判断映射关系是不是一致

​	在这里，第二个串变得更复杂了，不过基本思路可以这样处理，将第二个串，一个一个映射到某一个字母上去，



​	使用python很简单就实现了

```python
class Solution:
    def wordPattern(self, pattern, str):
        """
        :type pattern: str
        :type str: str
        :rtype: bool
        """
        
        temp = str.split(" ")
        if len(pattern) != len(temp):
            return False
        p_dict = {}
        s_dict = {}
        for i in range(len(pattern)):
            if not s_dict.get(temp[i]) :
                s_dict[temp[i]] = pattern[i]
            elif s_dict[temp[i]] != pattern[i]:
                return False

            if not p_dict.get(pattern[i]):
                p_dict[pattern[i]] = temp[i]
            elif p_dict[pattern[i]] != temp[i]:
                return False

        return True
        
```

