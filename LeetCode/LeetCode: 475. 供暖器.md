# LeetCode: 475. 供暖器

[TOC]

## 1、题目描述



冬季已经来临。 你的任务是设计一个有固定加热半径的供暖器向所有房屋供暖。

现在，给出位于一条水平线上的房屋和供暖器的位置，找到可以覆盖所有房屋的最小加热半径。

所以，你的输入将会是房屋和供暖器的位置。你将输出供暖器的最小加热半径。

**说明:**

1. 给出的房屋和供暖器的数目是非负数且不会超过 25000。
2. 给出的房屋和供暖器的位置均是非负数且不会超过10^9。
3. 只要房屋位于供暖器的半径内(包括在边缘上)，它就可以得到供暖。
4. 所有供暖器都遵循你的半径标准，加热的半径也一样。

**示例 1:**

```
输入: [1,2,3],[2]
输出: 1
解释: 仅在位置2上有一个供暖器。如果我们将加热半径设为1，那么所有房屋就都能得到供暖。
```

**示例 2:**

```
输入: [1,2,3,4],[1,4]
输出: 1
解释: 在位置1, 4上有两个供暖器。我们需要将加热半径设为1，这样所有房屋就都能得到供暖。
```



## 2、解题思路

​	将房间好供暖器排序，对于每一个房间，找到离它最小的供暖半径是多少

​	然后，所有的房间中，最小供暖半径中找到最大的就可以了

​	下面的写法很简单，但是会超出时间限制，所以需要换个思路

```c
int findRadius(int *houses, int housesSize, int *heaters, int heatersSize) {

    int result = INT32_MIN;
    int current_min = INT32_MAX;
    for (int i = 0; i < housesSize; i++) {
        for (int j = 0; j < heatersSize; j++) {
            if (abs(houses[i] - heaters[j]) < current_min) {
                current_min = abs(houses[i] - heaters[j]);
            }
        }

        if (result < current_min) {
            result = current_min;
        }

        current_min = INT32_MAX;
    }

    return result;
}
```





​	下面是更好一点的优化解决办法，通过将之排序，就能够在扫描第一个的时候，确定与供暖期的哪一个进行比较

​	

```c
int cmp(int *a, int *b) {
    return *a - *b;
}
int findRadius(int* houses, int housesSize, int* heaters, int heatersSize) {
    int result = INT32_MIN;
    qsort(houses, housesSize, sizeof(int), cmp);
    qsort(heaters, heatersSize, sizeof(int), cmp);

    int heart_pos = 0;
    for (int i = 0; i < housesSize; i++) {
        while (heart_pos < heatersSize - 1 &&
               abs(houses[i] - heaters[heart_pos]) >= abs(houses[i] - heaters[heart_pos + 1])) {
            heart_pos++;
        }
        result = result > abs(houses[i] - heaters[heart_pos]) ? result : abs(houses[i] - heaters[heart_pos]);

    }

    return result;
    
}
```



