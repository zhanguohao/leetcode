# LeetCode: 496. 下一个更大元素 I

[TOC]



## 1、题目描述



给定两个**没有重复元素**的数组 `nums1` 和 `nums2` ，其中`nums1` 是 `nums2` 的子集。找到 `nums1` 中每个元素在 `nums2` 中的下一个比其大的值。

`nums1` 中数字 **x** 的下一个更大元素是指 **x** 在 `nums2` 中对应位置的右边的第一个比 **x** 大的元素。如果不存在，对应位置输出-1。

**示例 1:**

```
输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。
```

**示例 2:**

```
输入: nums1 = [2,4], nums2 = [1,2,3,4].
输出: [3,-1]
解释:
    对于num1中的数字2，第二个数组中的下一个较大数字是3。
    对于num1中的数字4，第二个数组中没有下一个更大的数字，因此输出 -1。
```

**注意:**

1. `nums1`和`nums2`中所有元素是唯一的。
2. `nums1`和`nums2` 的数组大小都不超过1000。





## 2、解题思路

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* nextGreaterElement(int* findNums, int findNumsSize, int* nums, int numsSize, int* returnSize) {
    
    int *result = (int *) malloc(sizeof(int) * findNumsSize);
    *returnSize = findNumsSize;
    int findNum = -1;
    bool start_to_find = false;
    for (int i = 0; i < findNumsSize; i++) {
        for (int j = 0; j < numsSize; j++) {
            if (nums[j] == findNums[i]) {
                start_to_find = true;
            }
            if (start_to_find) {
                if (nums[j] > findNums[i]) {
                    findNum = nums[j];
                    break;
                }
            }
        }
        result[i] = findNum;
        findNum = -1;
        start_to_find = false;
    }

    return result;
    

}
```

