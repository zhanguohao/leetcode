# LeetCode: 859. 亲密字符串

[TOC]



## 1、题目描述

给定两个由小写字母构成的字符串 `A` 和 `B` ，只要我们可以通过交换 `A` 中的两个字母得到与 `B` 相等的结果，就返回 `true` ；否则返回 `false` 。

 

**示例 1：**

```
输入： A = "ab", B = "ba"
输出： true
```

**示例 2：**

```
输入： A = "ab", B = "ab"
输出： false
```

**示例 3:**

```
输入： A = "aa", B = "aa"
输出： true
```

**示例 4：**

```
输入： A = "aaaaaaabc", B = "aaaaaaacb"
输出： true
```

**示例 5：**

```
输入： A = "", B = "aa"
输出： false
```

 

**提示：**

1. `0 <= A.length <= 20000`
2. `0 <= B.length <= 20000`
3. `A` 和 `B` 仅由小写字母构成。



## 2、解题思路

​	这道题实际上考察的很简答，

- 首先判断，如果两个字符串长度不一致，直接返回false
- 长度一致的情况下，判断有几个位置的字符不一致
- 如果是2个，判断这两个交换以后是不是相同的，如果是，返回True
- 如果字符全部一致，判断是不是有两个相同的字符，有则返回True
- 其他情况返回False



```python
class Solution:
    def buddyStrings(self, A, B):
        """
        :type A: str
        :type B: str
        :rtype: bool
        """
        
        length_a = len(A)
        length_b = len(B)
        if length_a != length_b:
            return False
        buff = {}
        buff_count = {}
        for i in range(length_a):
            if A[i] != B[i]:
                buff[i] = (A[i], B[i])
            if buff_count.get(A[i]) is None:
                buff_count[A[i]] = 1
            else:
                buff_count[A[i]] += 1

        if len(buff) == 0:
            if len(buff_count) > 0 and  max(buff_count.values()) >= 2:
                return True

        if len(buff) == 2:
            temp = list(buff.values())
            if sorted(temp[0]) == sorted(temp[1]):
                return True
        return False
```

