# LeetCode: 448. 找到所有数组中消失的数字

[TOC]



## 1、题目描述



给定一个范围在  1 ≤ a[i] ≤ *n* ( *n* = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, *n*] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为*O(n)*的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

**示例:**

```
输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]
```



## 2、解题思路

​	根据上面的描述，我们知道，有一些元素重复出现了，如果我们将这些元素和下标关联起来，街能够找出来

​	例如，如果是数字2，就把[2-1]的下表的数字变成负数，从头向后扫描一遍，哪一个数字没有变成负数，就把[i+1]放到结果中

​	另一种，则是将对应下标的数加上n，如果有哪一个数最后没有被加，表示这个数就是缺失的

​	或者使用交换，将当前数与下标对应的数进行交换，交换了以后，判断对应下标的数字是不是下标+1，不是，表示这个数缺失



```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* findDisappearedNumbers(int* nums, int numsSize, int* returnSize) {
    int count = 0;
    int target;
    for (int i = 0; i < numsSize; i++) {
        target = nums[i] > 0 ? nums[i] : -nums[i];
        if (nums[target - 1] > 0) {
            count++;
            nums[target - 1] = -nums[target - 1];
        }

    }

    int *result = (int *) calloc(numsSize - count, sizeof(int));

    int pos = 0;
    for (int i = 0; i < numsSize; i++) {
        if (nums[i] > 0) {
            result[pos++] = i + 1;
        }
    }

    *returnSize = pos;

    return result;
}
```





