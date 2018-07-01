# LeetCode: 292.Nim游戏

[TOC]



## 1、题目描述

​	你和你的朋友，两个人一起玩 [Nim游戏](https://baike.baidu.com/item/Nim%E6%B8%B8%E6%88%8F/6737105)：桌子上有一堆石头，每次你们轮流拿掉 1 - 3 块石头。 拿掉最后一块石头的人就是获胜者。你作为先手。

你们是聪明人，每一步都是最优解。 编写一个函数，来判断你是否可以在给定石头数量的情况下赢得游戏。

**示例:**

```
输入: 4
输出: false 
解释: 如果堆中有 4 块石头，那么你永远不会赢得比赛；
     因为无论你拿走 1 块、2 块 还是 3 块石头，最后一块石头总是会被你的朋友拿走。
```



## 2、解题思路

我们定义Position

P：表示当前局面下先手必败

N：表示当前局面下先手必胜

N，P状态的转移满足如下性质：

1.合法操作集合为空的局面为P

2.可以移动到P的局面为N，这个很好理解，以为只要能转换到P局面，那么先手只需要使操作后变成P局面，那么后手就面临了一个必败的状态。

3.所有移动只能到达N的局面为P。无论怎么选取都会留给对手一个必胜状态。

其实知道这个之后应该是可以记忆化搜索或者用sg函数求解的，但是如果范围非常大，就没法做了。

就引进了nim游戏一个很神奇的结论：对于一个局面，当且仅当a[1] xor a[2] xor ...xor a[n]=0时，该局面为P局面，即必败局面。



​	根据上面的分析，我是先手，那么如果取1，2，3后产生的局面都是必赢局，那当前局面肯定是必输局

​	

​	如果1，2，3以后，产生了一个必败局，表示我们肯定赢了

### 2.1 递归法



​	遗憾的是，递归法太慢了，超出时间限制

```c
bool canWinNim(int n) {
    if (n <= 0) {
        return false;
    }

    if (n <= 3 && n >= 1) {
        return true;
    }

    
    
    return !canWinNim(n-1) || !canWinNim(n-2) || !canWinNim(n-3);

}
```



### 2.2 动态规划

​	如果我们将前面的结果存储起来，然后后面的结果通过前面的来判断，速度会快很多

​	不过依然是超过时间限制



```c
bool canWinNim(int n) {
    
    if (n == 0) {
        return false;
    }

    if (n <= 3) {
        return true;
    }

    bool *buff = (bool *) malloc(sizeof(bool) * (n + 1));
    // 初始化最初的状态
    buff[0] = false;
    buff[1] = true;
    buff[2] = true;
    buff[3] = true;

    for (int i = 4; i <= n; i++) {
        buff[i] = !buff[i - 1] || !buff[i - 2] || !buff[i - 3];
    }

    bool ans = buff[n];
    free(buff);
    return ans;

}
```





### 2.3  取余法



​	通过对多个数进行分析，发现结果以4不断地重复

```c
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>


bool canWinNim(int n) {
    if (n == 0) {
        return false;
    }

    if (n <= 3) {
        return true;
    }

    bool *buff = (bool *) malloc(sizeof(bool) * (n + 1));
    // 初始化最初的状态
    buff[0] = false;
    buff[1] = true;
    buff[2] = true;
    buff[3] = true;

    for (int i = 4; i <= n; i++) {
        buff[i] = !buff[i - 1] || !buff[i - 2] || !buff[i - 3];
    }

    bool ans = buff[n];
    free(buff);
    return ans;


}

int main() {
    for (int i = 0; i < 1000; i++) {
        printf("num: %d\t result: %s \n", i, canWinNim(i) ? "true" : "false");
    }
}
```



```
num: 0	 result: false 
num: 1	 result: true 
num: 2	 result: true 
num: 3	 result: true 
num: 4	 result: false 
num: 5	 result: true 
num: 6	 result: true 
num: 7	 result: true 
num: 8	 result: false 
num: 9	 result: true 
num: 10	 result: true 
num: 11	 result: true 
num: 12	 result: false 
num: 13	 result: true 
num: 14	 result: true 
num: 15	 result: true 
num: 16	 result: false 
num: 17	 result: true 
num: 18	 result: true 
num: 19	 result: true 
num: 20	 result: false 
num: 21	 result: true 
num: 22	 result: true 
num: 23	 result: true 
num: 24	 result: false 
num: 25	 result: true 
num: 26	 result: true 
num: 27	 result: true 
num: 28	 result: false 
num: 29	 result: true 
num: 30	 result: true 
num: 31	 result: true 
num: 32	 result: false 
num: 33	 result: true 
num: 34	 result: true 
num: 35	 result: true 
num: 36	 result: false 
num: 37	 result: true 
num: 38	 result: true 
num: 39	 result: true 
num: 40	 result: false 
num: 41	 result: true 
num: 42	 result: true 
num: 43	 result: true 
num: 44	 result: false 
num: 45	 result: true 
num: 46	 result: true 
num: 47	 result: true 
num: 48	 result: false 
num: 49	 result: true 
num: 50	 result: true 
num: 51	 result: true 
num: 52	 result: false 
num: 53	 result: true 
num: 54	 result: true 
```





所以，最终结果如下：

```c
bool canWinNim(int n) {
    return n%4;
}
```

