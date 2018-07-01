# LeetCode: 665. 非递减数列

[TOC]

## 1、题目描述



给定一个长度为 `n` 的整数数组，你的任务是判断在**最多**改变 `1` 个元素的情况下，该数组能否变成一个非递减数列。

我们是这样定义一个非递减数列的： 对于数组中所有的 `i` (1 <= i < n)，满足 `array[i] <= array[i + 1]`。

**示例 1:**

```
输入: [4,2,3]
输出: True
解释: 你可以通过把第一个4变成1来使得它成为一个非递减数列。
```

**示例 2:**

```
输入: [4,2,1]
输出: False
解释: 你不能在只改变一个元素的情况下将其变为非递减数列。
```

**说明:**  `n` 的范围为 [1, 10,000]。



## 2、解题思路

​	



- 设置两个指针，左面和右面的指向了左面第一个减小的位置，右面指向了右面第一个减小的位置
- 判断这两个中间，有几次递减机会，超过一次，返回false
- 如果只有一次，找出中间的最大值，最小值
- 最小值和左面的值进行比较，如果小于，并且最大值和右面的一个值比较，如果大于，这种情况是不可以的，返回false
- 其他的都是true





```c
bool checkPossibility(int* nums, int numsSize) {
    
    
    int left_pos = 0;
    int right_pos = numsSize - 1;

    for (int i = 0; i < numsSize - 1; i++) {
        if (nums[i + 1] < nums[i]) {
            left_pos = i;
            break;
        }
    }
    if (left_pos == numsSize - 1) {
        return true;
    }

    for (int i = numsSize - 1; i > 0; i--) {
        if (nums[i - 1] > nums[i]) {
            right_pos = i;
            break;
        }
    }


    // 找到区间的最大最小值
    int max = INT32_MIN;
    int min = INT32_MAX;
    int count = 0;
    for (int i = left_pos; i <= right_pos; i++) {
        if (nums[i] > max) {
            max = nums[i];
        }

        if (nums[i] < min) {
            min = nums[i];
        }

        if (i + 1 <= right_pos && nums[i + 1] < nums[i]) {
            count++;
        }

    }
    if (count >= 2) {
        return false;
    }

    if (left_pos > 0 && right_pos < numsSize - 1) {
        if (max > nums[right_pos + 1] && min < nums[left_pos - 1]) {
            return false;
        }
    }

    return true;

    
}
```





