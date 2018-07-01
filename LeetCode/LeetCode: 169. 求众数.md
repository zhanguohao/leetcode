# LeetCode: 169. 求众数

[TOC]

## 1、题目描述



给定一个大小为 *n* 的数组，找到其中的众数。众数是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。

**示例 1:**

```
输入: [3,2,3]
输出: 3
```

**示例 2:**

```
输入: [2,2,1,1,1,2,2]
输出: 2
```



## 2、解题思路

### 2.1 循环扫描法

​	使用双重循环，找到次数大于数组长度一般的元素，返回该值

​	代码没什么问题，但是在leetcode上面运行，发现超时了，超时的数组长度是50000，所以还需要使用其他方法

```c
int majorityElement(int* nums, int numsSize) {
    int count = 0;
    int result = 0;
    for (int i = 0; i <= numsSize / 2; i++) {
        result = nums[i];
        for (int j = 0; j < numsSize; j++) {
            if (nums[i] == nums[j]) {
                count++;
            }
        }
        if (count > numsSize / 2) {
            break;
        }
        count = 0;
    }

    return result;
}
```



​	

### 2.2 次数统计法

​	也称之为摩尔投票法

​	首先根据题意，假如每一个元素手里面有一票，只投给自己，其他元素都会给自己减一票，那么，最终剩下的，票数大于0的那个，肯定是超过半数的那个

​	

```c
int majorityElement(int* nums, int numsSize) {
    
    int count = 0;
    int result = nums[0];
    for (int i = 0; i < numsSize; i++) {
        if (count == 0) {
            result = nums[i];
            count = 1;
        } else {
            if (nums[i] == result) {
                count++;
            } else {
                count--;
            }
        }
    }

    return result;
    
   
}
```



### 

### 	 