# LeetCode: 167. 两数之和 II - 输入有序数组

[TOC]



## 1、题目描述

给定一个已按照**升序排列** 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2*。*

**说明:**

- 返回的下标值（index1 和 index2）不是从零开始的。
- 你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

**示例:**

```
输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```



## 2、解题思路

​	因为是升序数组，那么直接设置两个下标数，从两边进行逼近

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* numbers, int numbersSize, int target, int* returnSize) {
    
    int left_index = 1;
    int right_index = numbersSize;
    int temp = 0;
    for (int i = 0; i < numbersSize; i++) {
        temp =numbers[left_index - 1] + numbers[right_index - 1];
        if (temp == target) {
            break;
        } else if (temp > target) {
            right_index--;
        } else if (temp < target) {
            left_index++;
        }
    }

    int *result = (int*)malloc(sizeof(int)*2);
    result[0] = left_index;
    result[1] = right_index;
    *returnSize = 2;

    return result;
    
}
```

