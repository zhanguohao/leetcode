# LeetCode: 43. 字符串相乘

[TOC]



## 1、题目描述



给定两个以字符串形式表示的非负整数 `num1` 和 `num2`，返回 `num1` 和 `num2` 的乘积，它们的乘积也表示为字符串形式。

**示例 1:**

```
输入: num1 = "2", num2 = "3"
输出: "6"
```

**示例 2:**

```
输入: num1 = "123", num2 = "456"
输出: "56088"
```

**说明：**

1. `num1` 和 `num2` 的长度小于110。
2. `num1` 和 `num2` 只包含数字 `0-9`。
3. `num1` 和 `num2` 均不以零开头，除非是数字 0 本身。
4. **不能使用任何标准库的大数类型（比如 BigInteger）**或**直接将输入转换为整数来处理**。



## 2、解题思路

​	将每一个字符拿出来，放到一个数组中

​	然后，用第一个数的每一位，去乘以第二个数，累加到结果中去

```
	1 2 3
  4 5 6
```

​	最后，从结果中，找出第一个不为0的数字，从这个数字往后，转换为字符串，返回

​	注意，数字在结果中是反着存放的，需要从右端找出第一个不为0的数，然后转换为字符串返回

​	

​	基本思路是这样的

假设是123*456

首先，我们用3*456

然后，先是3*8，得到18，最低位就是8，进位是1

然后是3*5，得到15，加进位，16，第二位是6，进位是1

然后是3*4，得到12，加进位，得到13，第三位是3，进位是1



​	然后进行进位处理

​	进位加上下一个位的值，如果还有进位，继续传递

​	

然后是2*456

​	因为2是第二位了，所有2*6的位置要从第二位开始算起

​	

​	以此类推



```python
class Solution:
    def multiply(self, num1, num2):
        """
        :type num1: str
        :type num2: str
        :rtype: str
        """
        num1_list = [ord(v) - ord('0') for v in num1]
        num2_list = [ord(v) - ord('0') for v in num2]

        length1 = len(num1)
        length2 = len(num2)

        result = [0 for i in range(length1 + length2)]

        for i in range(length1):
            temp = 0
            carry = 0
            for j in range(length2):
                temp = result[i + j] + num1_list[length1 - 1 - i] * num2_list[length2 - 1 - j] + carry
                result[i + j] = temp % 10
                carry = temp // 10

            # 进位处理
            k = i + j + 1
            while carry != 0:
                temp = result[k] + carry
                result[k] = temp % 10
                carry = temp // 10
                k += 1

        index = length1 + length2 - 1
        while index >= 0:
            if result[index] == 0:
                index -= 1
            else:
                break

        if index == -1:
            return "0"

        return "".join([ chr(v + ord('0')) for v in   reversed(result[:index + 1])])
```



​	