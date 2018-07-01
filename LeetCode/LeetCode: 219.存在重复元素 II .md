# LeetCode: 219.存在重复元素 II 

[TOC]



## 1、题目描述

给定一个整数数组和一个整数 *k*，判断数组中是否存在两个不同的索引 *i* 和 *j*，使得 **nums [i] = nums [j]**，并且 *i* 和 *j* 的差的绝对值最大为 *k*。

**示例 1:**

```
输入: nums = [1,2,3,1], k = 3
输出: true
```

**示例 2:**

```
输入: nums = [1,0,1,1], k = 1
输出: true
```

**示例 3:**

```
输入: nums = [1,2,3,1,2,3], k = 2
输出: false
```



## 2、解题思路

​	

### 2.1 遍历法

​	这种方法比较慢

​	

```c
bool containsNearbyDuplicate(int* nums, int numsSize, int k) {
    if (!nums || numsSize <= 1) {
        return false;
    }

    for (int i = 0; i < numsSize; i++) {

        for (int j = 1; j <= k && (i + j) < numsSize; j++) {

            if (nums[i] == nums[i + j]) {
                return true;
            }
        }
    }
    return false;
    
    
    
    
}
```



### 2.2 缓存法

​	在最大值和最小值的差值，创建缓存数组，存储下标

```c
bool containsNearbyDuplicate(int* nums, int numsSize, int k) {
    if (!nums || numsSize <= 1) {
        return false;
    }

    int min = INT32_MAX;
    int max = INT32_MIN;

    for (int i = 0; i < numsSize; i++) {
        min = min > nums[i] ? nums[i] : min;
        max = max < nums[i] ? nums[i] : max;
    }

    int *buff_index = (int *) malloc(sizeof(int) * (max - min + 1));
    for (int i = 0; i < max - min + 1; ++i)
        buff_index[i] = -1;
    for (int i = 0; i < numsSize; i++) {
        if (buff_index[nums[i] - min] == -1) {
            buff_index[nums[i] - min] = i;
        } else {
            if (i - buff_index[nums[i] - min] <= k) {
                return true;
            } else {
                buff_index[nums[i] - min] = i;
            }
        }
    }

    return false;
    
    
}
```

