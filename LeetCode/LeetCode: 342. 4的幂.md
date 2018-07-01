# LeetCode: 342. 4的幂

[TOC]

## 1、题目描述



给定一个整数 (32位有符整数型)，请写出一个函数来检验它是否是4的幂。

**示例:**
当 num = 16 时 ，返回 true 。 当 num = 5时，返回 false。

**问题进阶：**你能不使用循环/递归来解决这个问题吗？

**致谢:**
特别感谢 [@yukuairoy ](https://leetcode.com/discuss/user/yukuairoy)添加这个问题并创建所有测试用例。



## 2、解题思路

​	从二进制位的角度来看，如果一个数是2的幂，n&(n-1)就能直接判断，也就是说只出现一个'1'

​	如果是4的幂，那么只需要在判断一次，1出现在偶数位上就行了



```cassandra
bool isPowerOfFour(int num) {
    if(num <=0){
        return false;
    }

    if(!(num&(num-1)) && 0x55555555 &num){
        return true;
    } else{
        return false;
    }
}
```

