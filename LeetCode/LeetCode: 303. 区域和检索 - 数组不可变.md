# LeetCode: 303. 区域和检索 - 数组不可变

[TOC]

## 1、题目描述



给定一个整数数组  *nums*，求出数组从索引 *i* 到 *j*  (*i* ≤ *j*) 范围内元素的总和，包含 *i,  j* 两点。

**示例：**

```
给定 nums = [-2, 0, 3, -5, 2, -1]，求和函数为 sumRange()

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

**说明:**

1. 你可以假设数组不可变。
2. 会多次调用 *sumRange* 方法



## 2、解题思路

​	使用一个缓冲数组，存放从第一个到当前所以的和

​	如果想要求一个范围，就用当前的减去对应的索引的值，加上前面索引处的值即可

```c
typedef struct {
    int *nums;
    int *buff;
} NumArray;

NumArray *numArrayCreate(int *nums, int numsSize) {
    NumArray *num = (NumArray *) malloc(sizeof(NumArray));
    num->nums = nums;
    num->buff = (int *) malloc(sizeof(int) * numsSize);
    int sum = 0;
    for (int i = 0; i < numsSize; i++) {
        sum += nums[i];
        num->buff[i] = sum;
    }
    return num;
}

int numArraySumRange(NumArray *obj, int i, int j) {
    return obj->buff[j] - obj->buff[i] + obj->nums[i];
}

void numArrayFree(NumArray *obj) {
    if (obj) {
        if (obj->buff) {
            free(obj->buff);
        }
        free(obj);
    }
}

/**
 * Your NumArray struct will be instantiated and called as such:
 * struct NumArray* obj = numArrayCreate(nums, numsSize);
 * int param_1 = numArraySumRange(obj, i, j);
 * numArrayFree(obj);
 */
```



​	略为改进

```c
typedef struct {
    int *nums;
    int *buff;
} NumArray;

NumArray *numArrayCreate(int *nums, int numsSize) {
    NumArray *num = (NumArray *) malloc(sizeof(NumArray));
    num->nums = nums;
    num->buff = (int *) malloc(sizeof(int) * numsSize);
    int sum = 0;
    for (int i = 0; i < numsSize; i++) {
        sum += nums[i];
        num->buff[i] = sum;
    }
    return num;
}

int numArraySumRange(NumArray *obj, int i, int j) {
    return obj->buff[j] - obj->buff[i] + obj->nums[i];
}

void numArrayFree(NumArray *obj) {
    if (obj) {
        if (obj->buff) {
            free(obj->buff);
        }
        free(obj);
    }
}
/**
 * Your NumArray struct will be instantiated and called as such:
 * struct NumArray* obj = numArrayCreate(nums, numsSize);
 * int param_1 = numArraySumRange(obj, i, j);
 * numArrayFree(obj);
 */
```

