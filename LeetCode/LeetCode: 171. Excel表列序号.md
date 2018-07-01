# LeetCode: 171. Excel表列序号

[TOC]

## 1、题目描述



给定一个Excel表格中的列名称，返回其相应的列序号。

例如，

```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

**示例 1:**

```
输入: "A"
输出: 1
```

**示例 2:**

```
输入: "AB"
输出: 28
```

**示例 3:**

```
输入: "ZY"
输出: 701
```



## 2、 解题思路

​	因为是从1开始的，因此，我们我们将这个数减去'A'，加上1，然后乘以26对应的阶数就得到了

```c
int titleToNumber(char* s) {
    if (!s) {
        return 0;
    }
    int length = strlen(s);
    int orderNUmber = 1;
    int result = 0;
    for (int i = length - 1; i >= 0; i--) {
        result += (s[i] - 'A' + 1) * orderNUmber;
        orderNUmber *= 26;

    }

    return result;
}
```



