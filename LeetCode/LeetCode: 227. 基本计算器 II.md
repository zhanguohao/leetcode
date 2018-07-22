# LeetCode: 227. 基本计算器 II

[TOC]



## 1、题目描述



实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，`+`， `-` ，`*`，`/` 四种运算符和空格 ` `。 整数除法仅保留整数部分。

**示例 1:**

```
输入: "3+2*2"
输出: 7
```

**示例 2:**

```
输入: " 3/2 "
输出: 1
```

**示例 3:**

```
输入: " 3+5 / 2 "
输出: 5
```

**说明：**

- 你可以假设所给定的表达式都是有效的。
- 请**不要**使用内置的库函数 `eval`。



## 2、解题思路

​	一般的，遇到这样的都能够使用栈来做，我们维护两个栈，一个栈存放数字，另一个栈存放运算符，因为乘法和除法的运算优先级比较高，那么我们在第一遍进行扫描的时候，就将乘除法计算出来，最后，运算符栈中剩下的就是加减法，这时候，每一次取出一个符号，就对数字栈的最后两个元素进行运算即可



```python
class Solution:
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """

        nums = []
        sign = []

        all_num = {"0": 0, "1": 1, "2": 2, "3": 3, "4": 4, "5": 5, "6": 6, "7": 7, "8": 8, "9": 9}
        all_signs = {"+": 10, "-": 11, "*": 12, "/": 13}

        calculate = False

        current_sign = -1

        for i in s:
            if calculate and i in all_num:
                if current_sign == all_signs["*"]:
                    nums[-1] = nums[-1] * all_num[i]

                else:
                    nums[-1] = nums[-1] // all_num[i]
                calculate = False
                continue
            if i in all_num:
                nums.append(all_num[i])

            if i in all_signs:
                if all_signs[i] < all_signs["*"]:
                    sign.append(all_signs[i])
                else:
                    current_sign = all_signs[i]
                    calculate = True

        for i in range(len(sign) - 1, -1, -1):
            num = nums.pop()
            if sign[i] == all_signs["+"]:
                nums[-1] = nums[-1] + num
            else:
                nums[-1] = nums[-1] - num

        return nums[0]


print(Solution().calculate("1+2+3*6"))

```

​	

​	直接写出来，思路没啥问题，不过没理解好，如果是多位数字的话，需要判断的。。。

​	上面写出来的是一位的数字的情况

​	如果有多位数的话，还是将表达式转换成后缀表达式再来做

```python
class Solution:
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        nums = []
        sign = []

        expression = []

        all_num = {"0": 0, "1": 1, "2": 2, "3": 3, "4": 4, "5": 5, "6": 6, "7": 7, "8": 8, "9": 9}
        all_signs = {"+": 10, "-": 11, "*": 12, "/": 13}
        low_priority = {"+": 10, "-": 11}
        high_priority = {"*": 12, "/": 13}

        is_num = False

        for i in s:
            if i in all_num:
                if is_num:
                    expression[-1] = expression[-1] + i
                else:
                    expression.append(i)
                    is_num = True

            if i in all_signs:
                is_num = False

                if sign:
                    if sign[-1] in high_priority:
                        expression.append(sign.pop())

                    if sign and i in low_priority:
                        expression.append(sign.pop())

                sign.append(i)

        expression.extend(reversed(sign))

        if len(expression) <= 0:
            return 0
        print(expression)

        for i in expression:
            if i not in all_signs:
                nums.append(int(i))
            else:
                num = nums.pop()
                if i == "+":
                    nums[-1] += num
                elif i == "-":
                    nums[-1] -= num
                elif i == "*":
                    nums[-1] *= num
                elif i == "/":
                    nums[-1] //= num

        return nums[-1]
```

​	

​	终于通过了，关于运算符的优先级的判断这需要注意

​	



