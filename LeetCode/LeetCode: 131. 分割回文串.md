# LeetCode: 131. 分割回文串

[TOC]

## 1、题目描述



给定一个字符串 *s*，将 *s* 分割成一些子串，使每个子串都是回文串。

返回 *s* 所有可能的分割方案。

**示例:**

```
输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]
```



## 2、解题思路

​	这道题是用递归来做，不过递归中嵌套这迭代，每一次需要判断是不是回文串，如果是，放到数组中，继续判断

​	如果不是，不用继续判断

```python
class Solution:
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """
    
    
        if not s:
            return [[]]

        result = []
        self.dfs(s, [], result)
        return result

    def dfs(self, s, current, result):
        if not s:
            result.append(current)
            return

        for i in range(len(s)):

            if self.isPalindrome(s[:i + 1]):
                temp = current[:]
                temp.append(s[:i + 1])
                self.dfs(s[i + 1:], temp, result)

    def isPalindrome(self, s):
        length = len(s)
        for i in range(length // 2):
            if s[i] != s[length - 1 - i]:
                return False

        return True
```

