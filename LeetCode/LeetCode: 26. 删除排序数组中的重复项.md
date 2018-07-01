# LeetCode: 26. 删除排序数组中的重复项

[TOC]



## 1、题目描述

```
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例 1:

给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。
示例 2:

给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。
说明:

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}

```



## 2、解题思路

​	设置一个不重复位置指针，另一个向前移动，每一次都判断之前的是不是重复

​	如果重复，当前指针直接加一

​	不重复，将不重复指针加一，并将当前值复制过去，然后当前指针加一



​	这种写法针对的所有数组，并不仅仅是排序数组

```c

int removeDuplicates(int *nums, int numsSize) {
    if (numsSize <= 1) {
        return numsSize;
    }
    int left_pos = 0;
    int cur_pos = 1;
    bool flag_same = false;
    while (numsSize-- > 1) {
        for (int i = left_pos; i >= 0; i--) {
            if (nums[cur_pos] == nums[i]) {
                flag_same = true;
                break;
            }
        }
        if (flag_same) {
            cur_pos++;
            flag_same = false;
        } else {
            nums[++left_pos] = nums[cur_pos];
            cur_pos++;

        }

    }

    return left_pos + 1;
}
```



下面的这种写法，仅仅针对于排序数组

```c
int removeDuplicates(int* nums, int numsSize) {
    if (numsSize <= 1){
        return numsSize;
    }
    int left_pos=0;

    for(int i = 1; i < numsSize; i++){
        if(nums[i] != nums[left_pos])
            nums[++left_pos] = nums[i];
    }
    return left_pos+1;
}
```

