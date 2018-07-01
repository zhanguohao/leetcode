# LeetCode: 643. 子数组最大平均数 I

[TOC]



## 1、题目描述



给定 `n` 个整数，找出平均数最大且长度为 `k` 的连续子数组，并输出该最大平均数。

**示例 1:**

```
输入: [1,12,-5,-6,50,3], k = 4
输出: 12.75
解释: 最大平均数 (12-5-6+50)/4 = 51/4 = 12.75
```

 

**注意:**

1. 1 <= `k` <= `n` <= 30,000。
2. 所给数据范围 [-10,000，10,000]。



## 2、解题思路

​	从左面向右扫描一遍即可

​	

```c
double findMaxAverage(int* nums, int numsSize, int k) {
    
    double result = INT32_MIN;
    double sum = 0;
    bool first = true;
    for (int i = 0; i <= numsSize - k; i++) {
        if (first) {
            for (int j = i; j < i + k; j++) {
                sum += nums[j];
            }
            first = false;
        } else {
            sum -= nums[i - 1];
            sum += nums[i + k - 1];
        }


        if (sum / k > result) {
            result = sum / k;
        }
    }

    return result;

}
```







​	