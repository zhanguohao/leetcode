# LeetCode: 784. 字母大小写全排列

[TOC]



## 1、题目描述





给定一个字符串`S`，通过将字符串`S`中的每个字母转变大小写，我们可以获得一个新的字符串。返回所有可能得到的字符串集合。

```
示例:
输入: S = "a1b2"
输出: ["a1b2", "a1B2", "A1b2", "A1B2"]

输入: S = "3z4"
输出: ["3z4", "3Z4"]

输入: S = "12345"
输出: ["12345"]
```

**注意：**

- `S` 的长度不超过`12`。
- `S` 仅由数字和字母组成。



```python
class Solution:
    def letterCasePermutation(self, S):
        """
        :type S: str
        :rtype: List[str]
        """
        
        result = [S]

        for i in range(len(S)):
            if S[i] >= 'a' and S[i] <= 'z':
                result += [s[:i] + chr(ord(s[i]) - 32) + s[i + 1:] for s in result]

            elif S[i] >= 'A' and S[i] <= 'Z':
                result += [s[:i] + chr(ord(s[i]) + 32) + s[i + 1:] for s in result]
        return result
        
```

