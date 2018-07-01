# LeetCode: 628. 三个数的最大乘积

[TOC]

## 1、题目描述



给定一个整型数组，在数组中找出由三个数组成的最大乘积，并输出这个乘积。

**示例 1:**

```
输入: [1,2,3]
输出: 6
```

**示例 2:**

```
输入: [1,2,3,4]
输出: 24
```

**注意:**

1. 给定的整型数组长度范围是[3,104]，数组中所有的元素范围是[-1000, 1000]。
2. 输入的数组中任意三个数的乘积不会超出32位有符号整数的范围。





## 2、解题思路

### 2.1 排序法

​	首先提议总是要得到最大的3个数的乘积，数据都是整型，因此，就会有两种情况

​	三个都是正数，2个负数一个正数

​	

简单的做法，直接排序，然后判断一下就好了

​	

```c
int cmp(int *a, int *b) {
    return *a - *b;
}

int maximumProduct(int *nums, int numsSize) {

    qsort(nums, numsSize, sizeof(int), cmp);

    int result_left = nums[0] * nums[1] * nums[numsSize - 1];
    int result_right = nums[numsSize - 3] * nums[numsSize - 2] * nums[numsSize - 1];

    return result_left > result_right ? result_left : result_right;


}

```



### 2.2 遍历法

​	通过遍历，找出最小的两个和最大的三个，比较最小的两个与最大的那个的乘积 与最大的3个数的乘积

