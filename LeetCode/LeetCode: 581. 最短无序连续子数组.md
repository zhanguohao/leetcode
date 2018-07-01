# LeetCode: 581. 最短无序连续子数组

[TOC]



## 1、题目描述



​	给定一个整数数组，你需要寻找一个**连续的子数组**，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是**最短**的，请输出它的长度。

**示例 1:**

```
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```

**说明 :**

1. 输入的数组长度范围在 [1, 10,000]。
2. 输入的数组可能包含**重复**元素 ，所以**升序**的意思是**<=。**





## 2、解题思路

​	乍一看，好像没什么思路，实际上，从题意中理解一下，假如我们已经知道了无序的那一段，剩下的就是有序的，也就是说，我虚的这一段肯定是在中间部分，两头的是有序的，也就是说，我们要找一个right，一个left，

​	该如何找到这个正确的左右界限呢？

​	首先，我们从左面向右面扫描，找left，只要遇到了第一个逆序，就将这个位置标记为left，跳出

​	这时候，判断left是不是在最后面，如果是，表示整个数组都是有序的，直接返回



​	然后从右面找right，遇到第一个逆序，将位置标记位right

​	接着，我们从left到right中间，找到最大值，最小值

​	最小值从0到left寻找，找到他的位置，用这个位置更新left

​	最大值从length到right寻找，更新right

​	结果就是right-left



```c
int findUnsortedSubarray(int* nums, int numsSize) {
    if (numsSize <= 1) {
        return 0;
    }

    int left = -1;
    for (int i = 0; i < numsSize - 1; i++) {
        if (nums[i] > nums[i + 1]) {
            left = i;
            break;
        }
    }
    // 表示数组是有序的
    if (left == -1) {
        return 0;
    }

    int right = numsSize;
    for (int i = numsSize - 1; i > 0; i--) {
        if (nums[i] < nums[i - 1]) {
            right = i;
            break;
        }
    }

    int min_value = nums[numsSize - 1];
    int max_value = nums[0];

    for (int i = left; i <= right; i++) {
        if (nums[i] < min_value) {
            min_value = nums[i];
        }

        if (nums[i] > max_value) {
            max_value = nums[i];
        }
    }

    int real_left = left;
    int real_right = right;
    // 确定left 和right真正的位置
    for (int i = 0; i <= left; i++) {
        if (min_value < nums[i]) {
            real_left = i;
            break;
        }
    }

    for (int i = numsSize - 1; i >= right; i--) {
        if (max_value > nums[i]) {
            real_right = i;
            break;
        }
    }

    return real_right - real_left + 1;
}
```

