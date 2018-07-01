# LeetCode: 49. 字母异位词分组

[TOC]



## 1、题目描述



给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。



## 2、解题思路

​	

```python
class Solution:
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        result = {}
        for i in strs:
            if result.get(tuple(sorted(i))) is None:
                result[tuple(sorted(i))] = [i]
            else:
                result[tuple(sorted(i))].append(i)
        return list(result.values())
```

