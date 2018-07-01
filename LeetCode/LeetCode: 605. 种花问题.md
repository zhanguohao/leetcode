# LeetCode: 605. 种花问题

[TOC]

## 1、题目描述



假设你有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花卉不能种植在相邻的地块上，它们会争夺水源，两者都会死去。

给定一个花坛（表示为一个数组包含0和1，其中0表示没种植花，1表示种植了花），和一个数 **n** 。能否在不打破种植规则的情况下种入 **n** 朵花？能则返回True，不能则返回False。



**示例 1:**

```
输入: flowerbed = [1,0,0,0,1], n = 1
输出: True
```

**示例 2:**

```
输入: flowerbed = [1,0,0,0,1], n = 2
输出: False
```

**注意:**

1. 数组内已种好的花不会违反种植规则。
2. 输入的数组长度范围为 [1, 20000]。
3. **n** 是非负整数，且不会超过输入数组的大小。



## 2、解题思路

​	实际上很简单，我们判断有多少个连续的0，然后判断连续的0的左右边界，如果左边界是1，右边界也是1，那么能够种下的花的数量要根据边界进行计算

​	例如，左边界是1，右边界是1，0的数量是3

​	能够种下的花的数量是1

​	如果0的数量是2，能够种下花的数量是0

​	实际上是（0的数量-左边界-右边界）/2 的值

​	考虑到奇偶数的问题，最终结果应该是：	（0的数量-左边界-右边界 +1）/2 的值

```c
bool canPlaceFlowers(int* flowerbed, int flowerbedSize, int n) {
    
    int left = 0;
    int zero_nums = 0;

    int result = 0;
    bool start_count = false;
    for (int i = 0; i < flowerbedSize; i++) {
        if (!start_count) {
            if (flowerbed[i] == 0) {
                start_count = true;
                zero_nums++;
                if (i == 0) {
                    left = 0;
                } else {
                    left = 1;
                }

            }
        } else {
            if (flowerbed[i] == 0) {
                zero_nums++;
            } else {
                result += (zero_nums - left) / 2;
                left = 0;
                zero_nums = 0;
                start_count = false;
            }
        }
    }

    if (flowerbed[flowerbedSize - 1] == 0) {
        result += (zero_nums - left + 1) / 2;
    }

    if (result >= n) {
        return true;
    } else {
        return false;
    }
}
```



