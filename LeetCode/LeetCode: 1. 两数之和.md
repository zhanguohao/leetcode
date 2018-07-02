# LeetCode: 1. 两数之和

[TOC]

## 1、题目描述

```
给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。

你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```



## 2、解题思路

### 2.1 解题1

​	一般我们第一时间就能想到的思路，就是用双重for循环来遍历一遍，肯定可以得到结果；

```
循环1：元素1 in 数组
	循环2：元素2 in 数组
		判断：元素1与元素2不是同一个元素 and 它们之和等于目标值：
			返回元素1和元素2的下标
```

```c
int* twoSum(int* nums, int numsSize, int target) {
    int* result = (int*)malloc(sizeof(int)*2);
    for (int i = 0;i< numsSize-1; i++){
            for(int j = i+1; j <numsSize;j++ ){
                if (nums[i] + nums[j] == target){
                    result[0] = i;
                    result[1] = j;
                    return result;
                }
            }   
    }
    return result;
}
```



### 2.2 解题2

​	双重for循环会浪费时间和空间，并且有这样的一个缺点，就是已经计算过的值，并不会保存，每一次的计算，与前面的计算没有关系，如果我们每一次计算都能利用前面计算的结果，那么效率应该会提高。

```
创建一个容器：buff  #使用buff存储下标和目标值与当前值的差值
循环： 元素 in 数组
	判断： 如果当前值在容器中存在
		返回：容器中的值下标，当前下标
	判断： 不存在
		将当前值得下标与(目标值-当前值)存储倒容器中
```

​	实际上，这里利用了一个缓冲容器的思路，将差值作为键，下标作为值存储到字典中，那么实际上我们只需要用一层循环就可以了，每一次都将前面元素的差值存储，那么每遇到一个元素，判断一下这个值在是不是在字典中，如果在，那么这个值与字典中的下标所代表的值之和肯定就是目标值，时间复杂度是O(n)。

## 3、参考代码

```
Python版本：3
```

```python
class Solution(object):
    def twoSum(self, nums, target):
        buff_dict = {}
        for i in range(len(nums)):
            if nums[i] in buff_dict:
                return [buff_dict[nums[i]], i]
            else:
                buff_dict[target - nums[i]] = i
```




