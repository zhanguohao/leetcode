# LeetCode: 268.缺失数字 

[TOC]

## 1、题目描述



给定一个包含 `0, 1, 2, ..., n` 中 *n* 个数的序列，找出 0 .. *n* 中没有出现在序列中的那个数。

**示例 1:**

```
输入: [3,0,1]
输出: 2
```

**示例 2:**

```
输入: [9,6,4,2,3,5,7,0,1]
输出: 8
```

**说明:**
你的算法应具有线性时间复杂度。你能否仅使用额外常数空间来实现?





## 2、解题思路

### 2.1 异或法

​	所有的数进行异或，确实的也进行异或

​	用确实的异或每一个数，判断是不是相等，相等返回true

```c
int missingNumber(int* nums, int numsSize) {
    
    if (!nums) {
        return -1;
    }
    int total = 0;
    int current = nums[0];

    for (int i = 1; i < numsSize + 1; i++) {
        total ^= i;
    }

    for (int i = 1; i < numsSize; i++) {
        current ^= nums[i];
    }

    for (int i = 0; i < numsSize + 1; i++) {
        if ((current ^ i) == total) {
            return i;
        }
    }

    return -1;  

}
```



### 2.2 加和法

​	将缺少的数组加起来，算出所有的数，减去可得

```cassandra
int missingNumber(int* nums, int numsSize) {
    
    int total = 0;
    for (int i = 0; i < numsSize; i++) {
        total += nums[i];
    }

    return (numsSize + 1) * numsSize / 2 - total;
    
    

}
```

