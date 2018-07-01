# LeetCode: 53.最大子序和

[TOC]

## 1、题目描述

```
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。
```



## 2、解题思路

​	从前向后扫描，如果加上当前值使得和小于0，将和更新为0，继续扫描

​	需要判断一种特殊情况，全部小于0的时候，返回最大的负数

```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <memory.h>
#include <string.h>


int maxSubArray(int *nums, int numsSize) {
    bool all_minux = true;
    int max_minus = INT32_MIN;
    int this = 0;
    int max = 0;
    for (int i = 0; i < numsSize; i++) {
        if (nums[i] >= 0){
            all_minux = false;
        }
        if(nums[i] <0 && max_minus < nums[i]){
            max_minus = nums[i];
        }
        this += nums[i];
        if (this > max) {
            max = this;
        }
        if (this < 0) {
            this = 0;
        }
    }
    if (all_minux){
        max = max_minus;
    }
    return max;

}

void print(int x) {
    int num = 1;
    for (int i = 31; i >= 0; i--) {
        if (x & (num << i)) {
            printf("1");
        } else {
            printf("0");
        }
    }
    printf("\n");
}

int main() {

    int a[]= {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    int b[]= {-2, -1, -3, -4, -1, -2, -1, -5, -4};
    printf("%d\n", maxSubArray(a, 9));
    printf("%d\n", maxSubArray(b, 9));


}

```

