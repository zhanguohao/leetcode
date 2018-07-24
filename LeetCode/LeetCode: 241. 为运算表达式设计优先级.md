# LeetCode: 241. 为运算表达式设计优先级

[TOC]



## 1、题目描述



给定一个含有数字和运算符的字符串，为表达式添加括号，改变其运算优先级以求出不同的结果。你需要给出所有可能的组合的结果。有效的运算符号包含 `+`, `-` 以及 `*` 。

**示例 1:**

```
输入: "2-1-1"
输出: [0, 2]
解释: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```

**示例 2:**

```
输入: "2*3-4*5"
输出: [-34, -14, -10, -10, 10]
解释: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```



## 2、解题思路

​	这道题目是一个分治法的典型应用，将大的问题分解成小问题求解

- 以每一个分隔符进行分割，变成两个子串，结果就是两个子串的值之和
- 因为每个子串都有多个值，我们要对第一个字串中的每个值进行匹配第二个子串值



例如

```
"2-1-1"
```

第一步，分割成

```
"2"   "1-1"
当前符号 "-"
得到左面的值[2]
得到右面的值[0]
组合得到[2]
```

第二次分割

```
"2-1"  "1"
当前符号   "-"
得到左面的值[1]
得到右面的值[1]
组合得到[0]
```

将组合得到的数字放到结果集中

```
[2 0]
```



```python
class Solution:
    def diffWaysToCompute(self, input):
        """
        :type input: str
        :rtype: List[int]
        """
        if not input:
            return []

        ans = []

        signs = {'+': 1, '-': 2, '*': 3}

        for i in range(len(input)):
            if input[i] in signs:
                left = self.diffWaysToCompute(input[:i])
                right = self.diffWaysToCompute(input[i + 1:])

                for m in left:
                    for n in right:
                        if signs[input[i]] == 1:
                            ans.append(m + n)
                        elif signs[input[i]] == 2:
                            ans.append(m - n)
                        elif signs[input[i]] == 3:
                            ans.append(m * n)

        if not ans:
            ans.append(int(input))

        return ans
```



