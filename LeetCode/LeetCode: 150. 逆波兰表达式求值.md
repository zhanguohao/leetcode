# LeetCode: 150. 逆波兰表达式求值

[TOC]



## 1、题目描述

------

根据[逆波兰表示法](https://baike.baidu.com/item/%E9%80%86%E6%B3%A2%E5%85%B0%E5%BC%8F/128437)，求表达式的值。

有效的运算符包括 `+`, `-`, `*`, `/` 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

**说明：**

- 整数除法只保留整数部分。
- 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

**示例 1：**

```
输入: ["2", "1", "+", "3", "*"]
输出: 9
解释: ((2 + 1) * 3) = 9
```

**示例 2：**

```
输入: ["4", "13", "5", "/", "+"]
输出: 6
解释: (4 + (13 / 5)) = 6
```

**示例 3：**

```
输入: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
输出: 22
解释: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```



## 2、解题思路

​	逆波兰表达式就是后缀表达式，直接利用栈求解即可

​	有几个细节需要注意，数字符号的判断

​	还有如果只有一个数字，需要将结果转换为int

```python
class Solution:
    def evalRPN(self, tokens):
        """
        :type tokens: List[str]
        :rtype: int
        """
        
        stack = []

        for i in tokens:
            if i.isalnum() or i[0] == '-' and i[1:].isalnum():
                stack.append(i)
            else:
                num1 = int(stack.pop())
                num2 = int(stack.pop())

                if i == '+':
                    stack.append(num1 + num2)
                elif i == '-':
                    stack.append(num2 - num1)
                elif i == '*':
                    stack.append(num1 * num2)
                elif i == '/':
                    stack.append(num2 / num1)
        return int(stack.pop())
        
        
```

