# LeetCode: 187. 重复的DNA序列

[TOC]

## 1、题目描述



所有 DNA 由一系列缩写为 A，C，G 和 T 的核苷酸组成，例如：“ACGAATTCCG”。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来查找 DNA 分子中所有出现超多一次的10个字母长的序列（子串）。

**示例:**

```
输入: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

输出: ["AAAAACCCCC", "CCCCCAAAAA"]
```



## 2、解题思路

​	直接用哈希来做，很简单

```python
class Solution(object):
    def findRepeatedDnaSequences(self, s):
        """
        :type s: str
        :rtype: List[str]
        """

        length = len(s)

        hash_buff = {}
        for i in range(length - 9):
            if hash_buff.get(s[i:i + 10]) is None:
                hash_buff[s[i:i + 10]] = 1
            else:
                hash_buff[s[i:i + 10]] += 1

        return [ k for k,v in hash_buff.items() if v >= 2]
```

