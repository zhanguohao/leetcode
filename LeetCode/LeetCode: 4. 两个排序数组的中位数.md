# LeetCode: 4. 两个排序数组的中位数

[TOC]

```
两个排序数组的中位数
```



## 1、问题描述

```
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

Example 1:
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
Example 2:
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

​	给定两个大小为 m 和 n 的有序数组 **nums1** 和 **nums2** 。

请找出这两个有序数组的中位数。要求算法的时间复杂度为 O(log (m+n)) 。

**示例 1:**

```
nums1 = [1, 3]
nums2 = [2]

中位数是 2.0
```

**示例 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

中位数是 (2 + 3)/2 = 2.5
```



## 2、解题思路

### 2.1 使用二分查找法

​	

```c
double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size) {
/*
*   基本思想：
*       每一次判断，要找的是第k个数，如果在数组1中，第k/2个数比数组个数还要大，就是用最后一个数
        用两个数组中的第k/2个数进行比较，如果第一个数组中的比第二个数组中的大，就能够将第二个数组中前k/2个数排除在比较范围之外
        这时候，要查找的的就不是第k个数，而是k-k/2个数，
*
*
*   left1：表示数组1的左边界，数组下标
*   left2：表示数组2的左边界，数组下标
*/
    int mid_loc;
    int pos1, pos2, left1 = 0, left2 = 0;
    // 奇偶判断，奇数为真，偶数为假
    bool flag_odd_even = (nums1Size + nums2Size) % 2 == 0 ? false : true;
    bool found = false;
    double result;
    int result_pos;
    // 根据奇偶数，判断中位数是第几位数
    if (flag_odd_even) {
        //如果是奇数
        mid_loc = (nums1Size + nums2Size) / 2 + 1;
    } else {
        //如果是偶数
        mid_loc = (nums1Size + nums2Size) / 2;
    }

    // 判断几种特殊情况，可以直接返回结果
    if (nums1Size == 0 && nums2Size != 0 && nums2Size > mid_loc) {
        if (flag_odd_even) {
            result = nums2[mid_loc - 1];
        } else {
            result = (1.0 * nums2[mid_loc - 1] + nums2[mid_loc]) / 2;
        }
    } else if (nums2Size == 0 && nums1Size != 0 && nums1Size > mid_loc) {
        if (flag_odd_even) {
            result = nums1[mid_loc - 1];
        } else {
            result = (1.0 * nums1[mid_loc - 1] + nums1[mid_loc]) / 2;

        }
    } else {
        // 使用二分法，进行判断
        while (!found) {

            pos1 = (mid_loc / 2) > (nums1Size - left1 - 1) ? nums1Size - 1 : (mid_loc / 2 + left1 - 1);
            pos2 = (mid_loc / 2) > (nums2Size - left2 - 1) ? nums2Size - 1 : (mid_loc / 2 + left2 - 1);

            if (nums1[pos1] >= nums2[pos2]) {
                // 如果第二个已经到了边界， 在第一个数组中直接找到结果
                if (pos2 == (nums2Size - 1)) {
                    result_pos = mid_loc - (pos2 - left2 + 1) + left1 - 1;
                    //奇数
                    if (flag_odd_even) {

                        result = nums1[result_pos];
                    } else {
                        result = (1.0 * nums1[result_pos] + nums1[result_pos + 1]) / 2;
                    }
                    found = true;
                } else {

                    mid_loc -= (pos2 - left2 + 1);
                    left2 = pos2 + 1;
                }
            } else {
                // 如果第一个到了边界，直接在第二个数组中找到结果
                if (pos1 == (nums1Size - 1)) {
                    result_pos = mid_loc - (pos1 - left1 + 1) + left2 - 1;
                    //奇数
                    if (flag_odd_even) {
                        result = nums2[result_pos];
                    } else {
                        result = (1.0 * nums2[result_pos] + nums2[result_pos + 1]) / 2;
                    }
                    found = true;

                } else {
                    mid_loc -= (pos1 - left1 + 1);
                    left1 = pos1 + 1;
                }
            }
            if (!found && mid_loc == 1) {

                if (nums1[left1] <= nums2[left2]) {
                    // 如果是奇数
                    if (flag_odd_even) {
                        result = nums1[left1];
                    } else {
                        // 第一个条件是边界保护
                        if ((left1 < nums1Size - 1) && (nums1[left1 + 1] <= nums2[left2])) {
                            result = (1.0 * nums1[left1] + nums1[left1 + 1]) / 2;
                        } else {
                            result = (1.0 * nums1[left1] + nums2[left2]) / 2;
                        }
                    }
                } else {
                    if (flag_odd_even) {
                        result = nums2[left2];
                    } else {
                        // 第一个条件是边界保护
                        if ((left2 < nums2Size - 1) && (nums2[left2 + 1] <= nums1[left1])) {
                            result = (1.0 * nums2[left2] + nums2[left2 + 1]) / 2;
                        } else {
                            result = (1.0 * nums2[left2] + nums1[left1]) / 2;

                        }
                    }

                }
                found = true;
            }
        }
    }
    return result;
}
```



### 2.2 缓冲数组法

​	这个缓冲法就有点笨拙了，就类似于归并排序里面的，将两个已排好序的数组合并起来，不过我们只需要其中一半即可，这样就找到中位数了，这种办法的好处就是思路非常清晰，简单，但是会占用额外的空间，并且速度不快；

```c
double findMedianSortedArrays(int *nums1, int nums1Size, int *nums2, int nums2Size) {
/*
*   基本思想：
*
*
*/

    int buff_length = (nums1Size + nums2Size) / 2 + 1;
    int num = (int*)malloc(sizeof(int)*buff_length);
    bool flag_odd_even = (nums1Size + nums2Size) % 2 == 0 ? false : true;
    int i=0,j=0;
    int count = 0;
    double result;
    int temp_length = buff_length;
    while (temp_length-- ){
        if ((i <nums1Size && nums1[i] <= nums2[j]) || j>= nums2Size ){
            num[count++] = nums1[i];
            i++;
        }else if((j < nums2Size && nums2[j] <= nums1[i]) || i >= nums1Size){
            num[count++] = nums2[j];
            j++;
        }
    }

    if (flag_odd_even){
        result = num[buff_length-1];
    } else{
        result = (1.0 * num[buff_length-2] + num[buff_length-1])/2;
    }

    return result;

}
```

