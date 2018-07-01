# LeetCode: 645. 错误的集合

[TOC]



## 1、题目描述

​	



集合 `S` 包含从1到 `n` 的整数。不幸的是，因为数据错误，导致集合里面某一个元素复制了成了集合里面的另外一个元素的值，导致集合丢失了一个整数并且有一个元素重复。

给定一个数组 `nums` 代表了集合 `S` 发生错误后的结果。你的任务是首先寻找到重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。

**示例 1:**

```
输入: nums = [1,2,2,4]
输出: [2,3]
```

**注意:**

1. 给定数组的长度范围是 [2, 10000]。
2. 给定的数组是无序的。



## 2、解题思路

​	前面做过一道类似的题目，我们直接让对应的数，让其下标加一个负号，如果发现某一个下标已经是负号的时候，这个元素就是重复元素

​	唯一的一个正元素的下表加1，就是缺失元素



​	解题思路与448题相似

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* findErrorNums(int* nums, int numsSize, int* returnSize) {
    
     int *result = (int *) malloc(sizeof(int) * 2);
    *returnSize = 2;


    for (int i = 0; i < numsSize; i++) {
        if (nums[abs(nums[i]) - 1] > 0) {
            nums[abs(nums[i]) - 1] = -nums[abs(nums[i]) - 1];
        } else {
            result[0] = abs(nums[i]);
        }
    }

    for (int i = 0; i < numsSize; i++) {
        if (nums[i] > 0) {
            result[1] = i + 1;
            break;
        }
    }

    return result;
}
```

