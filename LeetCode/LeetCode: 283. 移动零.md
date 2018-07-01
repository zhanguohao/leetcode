# LeetCode: 283. 移动零

[TOC]

## 1、题目描述



给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例:**

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**说明**:

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。



## 2、解题思路



​	分两步，第一次将所有的非0的数向前移动到正确的位置，

​	第二步，将后面的数赋值0



```c
void moveZeroes(int* nums, int numsSize) {
//     int zero_count = 0;
//     int non_zero_pos = 0;

//     for (int i = 0; i < numsSize; i++) {
//         if (nums[i] == 0) {
//             zero_count++;
//         } else {
//             nums[non_zero_pos] = nums[i];
//             non_zero_pos++;
//         }

//     }

//     for (int i = 0; i < zero_count; i++) {
//         nums[non_zero_pos++] = 0;
//     }
    
    
    int non_zero_pos = 0;

    for (int i = 0; i < numsSize; i++) {
        if (nums[i]) {
            nums[non_zero_pos++] = nums[i];
        }

    }

    for (int i = non_zero_pos; i < numsSize; i++) {
        nums[non_zero_pos++] = 0;
    }
    
}
```

