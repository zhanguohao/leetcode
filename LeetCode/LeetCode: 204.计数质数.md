# LeetCode: 204.计数质数

[TOC]

## 1、题目描述

统计所有小于非负整数 *n* 的质数的数量。

**示例:**

```
输入: 10
输出: 4
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```



## 2、解题思路

### 2.1 链表缓存

​	使用一个链表，缓存所有小于n的质数，判断质数的时候，用这些质数判断

​	不够遗憾的是，在leetcode上，这种方法时间太长了

==不通过==

```c
int countPrimes(int n) {
    if (n <= 2) {
        return 0;
    }
    // 2 直接写到里面去
    int count = 1;

    struct ListNode *primes = (struct ListNode *) malloc(sizeof(struct ListNode));
    struct ListNode *tail = primes;
    struct ListNode *temp = primes;

    primes->val = 2;
    primes->next = NULL;

    bool flag;
    for (int i = 3; i < n; i++) {
        flag = true;
        while (temp) {
            if (i % temp->val == 0) {
                flag = false;
                break;
            }
            temp = temp->next;
        }

        if (flag) {
            tail->next = (struct ListNode *) malloc(sizeof(struct ListNode));
            tail = tail->next;
            tail->val = i;
            tail->next = NULL;
            count++;
        }

        temp = primes;
    }

    temp = primes->next;
    while (temp) {
        free(primes);
        primes = temp;
        temp = temp->next;
    }
    free(primes);

    return count;
}
```



### 2.2 标记法

​	创建一个数组，每一次将2的倍数，倍数，5的倍数依次进行标记，直到标记到这个数为止

​	统计有多少没有被标记的，得到结果

​        ==埃拉托斯特尼筛法 Sieve of Eratosthenes==



```c
int countPrimes(int n) {
    if (n <= 2) {
        return 0;
    }


    bool buf[n + 1];
    memset(buf,true,n+1);
    buf[0] = false;
    buf[1] = false;
    // 这个是哨兵
    buf[n] = true;

    int count = 0;
    int sign_pos = 2;

    while (sign_pos < n) {
        // 将sign_pos所有的倍数都标记为false
        for (int i = 2 * sign_pos; i < n; i += sign_pos) {
            if (i % sign_pos == 0) {
                buf[i] = false;
            }
        }
        buf[n] = true;

        while (!buf[++sign_pos]);

    }


    buf[n] = false;
    for (int i = 0; i < n; i++) {
        if (buf[i]) {
            count++;
        }
    }

    return count;
    

}
```





