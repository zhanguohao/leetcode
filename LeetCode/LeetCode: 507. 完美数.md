# LeetCode: 507. 完美数

[TOC]



## 1、题目描述



对于一个 **正整数**，如果它和除了它自身以外的所有正因子之和相等，我们称它为“完美数”。

给定一个 **正整数** `n`， 如果他是完美数，返回 `True`，否则返回 `False`

 

**示例：**

```
输入: 28
输出: True
解释: 28 = 1 + 2 + 4 + 7 + 14
```



## 2、解题思路

```c
bool checkPerfectNumber(int num) {
    
    if (num == 1 || num == 0) {
        return false;
    }


    int ans = 1;

    int temp;
    int sq = (int) sqrt(num);
    for (int i = 2; i <= sq; i++) {
        temp = num % i;
        if (temp == 0) {
            ans += num / i + i;
        }
    }


     if (sq != 1 && num / sq == sq && num %sq == 0) {
        num -= sq;
    }
    

    if (num == ans) {
        return true;
    } else {
        return false;
    }
    
    
}
```

