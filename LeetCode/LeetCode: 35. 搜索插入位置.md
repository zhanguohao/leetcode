# LeetCode: 35. 搜索插入位置

[TOC]



## 1、题目描述

```
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

示例 1:

输入: [1,3,5,6], 5
输出: 2
示例 2:

输入: [1,3,5,6], 2
输出: 1
示例 3:

输入: [1,3,5,6], 7
输出: 4
示例 4:

输入: [1,3,5,6], 0
输出: 0
```



## 2、解题思路

### 2.1 依次扫描法

​	从前向后扫描，遇到比当前数字小的，就将下标返回

```c
int searchInsert(int* nums, int numsSize, int target) {
    int result_pos = 0;
	for(int i = 0;i<numsSize; i++ ){
		if(target > nums[i]){
			result_pos++;
		}else{
            return result_pos;
        }
	}
	return result_pos;
}
```



## 2.2 二分法

​	从中间开始比较，加快搜索速度

​	每一次更新左右的范围

​	最后找到的那个比较一次，即可得到下标

```c
int searchInsert(int* nums, int numsSize, int target) {
 int left = 0;
	int right = numsSize-1;
	while(left<right){
		// 如果目标值大于中间值
		if (target > nums[(left+right)/2]){
			left = (left+right)/2 +1;
		}else{
			right = (left+right)/2;
		}
	}
	if (target > nums[left]){
		left++;
	}
	return left;
}
```

