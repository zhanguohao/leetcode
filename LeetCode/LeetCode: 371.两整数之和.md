# LeetCode: 371.两整数之和

[TOC]



## 1、题目描述





**不使用**运算符 `+` 和`-`，计算两整数`a` 、`b`之和。

**示例：**
若 *a* = 1 ，*b* = 2，返回 3。

**致谢：**
特别感谢 [@fujiaozhu](https://discuss.leetcode.com/user/fujiaozhu) 添加这道问题并创建测试用例。



## 2、解题思路

- 使用异或来做，
- 通过异或可以得到结果，通过与得到进位
- 将进位左移，作为新的加数
- 直到进位变成0为止



```c
int getSum(int a, int b) {
    int carry = a & b;
    int result = a ^b;
    int temp;
    while (carry != 0) {
        temp = result;
        result = result ^ (carry << 1);
        carry = temp & (carry << 1);
    }
    return result;
}
```



