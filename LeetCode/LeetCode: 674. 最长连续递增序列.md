# LeetCode: 674. 最长连续递增序列

[TOC]



## 1、题目描述



给定一个未经排序的整数数组，找到最长且**连续**的的递增序列。

**示例 1:**

```
输入: [1,3,5,4,7]
输出: 3
解释: 最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为5和7在原数组里被4隔开。 
```

**示例 2:**

```
输入: [2,2,2,2,2]
输出: 1
解释: 最长连续递增序列是 [2], 长度为1。
```

**注意：**数组长度不会超过10000。



## 2、解题思路

​	

​	注意理解题意，一开始想的有点复杂了，只要是升序就可以，一开始还以为必须是间隔相同的升序



```c
int findLengthOfLCIS(int* nums, int numsSize) {
    if (numsSize <= 1) {
        return numsSize;
    }

    int left = 0;
    int result = 0;

    int current_interval;

    for (int i = 0; i < numsSize - 1; i++) {
        current_interval = nums[i + 1] - nums[i];
        if (current_interval <= 0) {
            result = result > i - left + 1 ? result : i - left + 1;
            left = i + 1;
        }
    }
    result = result > numsSize - left ? result : numsSize - left;

    return result;
}
```

