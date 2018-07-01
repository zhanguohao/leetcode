# LeetCode: 485. 最大连续1的个数

[TOC]

## 1、题目描述



给定一个二进制数组， 计算其中最大连续1的个数。

**示例 1:**

```
输入: [1,1,0,1,1,1]
输出: 3
解释: 开头的两位和最后的三位都是连续1，所以最大连续1的个数是 3.
```

**注意：**

- 输入的数组只包含 `0` 和`1`。
- 输入数组的长度是正整数，且不超过 10,000。



## 2、解题思路



```c
int findMaxConsecutiveOnes(int* nums, int numsSize) {
    
     int onesNum = 0;
    int result = INT32_MIN;


    for (int i = 0; i < numsSize; i++) {
        if (onesNum == 0) {
            if (nums[i] == 1) {
                onesNum++;
            } else {
                continue;
            }
        } else {

            if (nums[i] == 0) {
                result = result > onesNum ? result : onesNum;
                onesNum = 0;
            } else {
                onesNum++;
            }
        }
    }

    result = result > onesNum ? result : onesNum;
    return result;

}
```

