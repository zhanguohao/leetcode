# LeetCode: 22. 括号生成

[TOC]

## 1、题目描述



给出 *n* 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且**有效的**括号组合。

例如，给出 *n* = 3，生成结果为：

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```



## 2、解题思路

​	

首先，我们来找一下规律

1：

​	()

2:

​	()()

​	(())

3:

​	()()()

​	(()())

​	(())()

​	()(())

​	((()))



​	

我们可以这样考虑，设置计数器，也就是做括号与右括号的计数，

首先，如果左右括号数相等，添加左括号

如果左括号大于右括号数并且小于n，两种情况，添加左括号，添加右括号



最后，如果左括号数等于右括号数，并且和为2n，添加到结果集中



```python
class Solution:
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        
        if n <= 0:
            return []

        result = set()

        self.generate("", n, 0, 0, result)
        return list(result)

    def generate(self, current, n, left, right, ans):
        if left + right == 2 * n:
            ans.add(current)
            return

        if left < n:
            self.generate(current + "(", n, left + 1, right, ans)
            if left > right:
                self.generate(current + ")", n, left, right + 1, ans)
        else:
            self.generate(current + ")", n, left, right + 1, ans)
        
```

