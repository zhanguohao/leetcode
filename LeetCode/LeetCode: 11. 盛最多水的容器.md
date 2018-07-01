# LeetCode: 11. 盛最多水的容器

[TOC]



## 1、题目描述



​	给定 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。画 *n* 条垂直线，使得垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**注意：**你不能倾斜容器，*n* 至少是2。



## 2、解题思路

​	一般能直接想到的就是暴力破解法，但是这个效率实在太差了

​	换个思路的话，我们从两边开始找起

​	考虑到盛水的容器仅与段的那个数字相关，我们设置两个指针，指向两头，然后不断地向中间移动，直到两个相遇为止，每一次都是小的边向长的边移动靠拢，这样能保证我们尽可能减少损失，找出最大的面积

​	

```cassandra
#define  min(a, b) (a<b?a:b)
#define  max(a, b) (a>b?a:b)


int maxArea(int* height, int heightSize) {
    int left = 0;
    int right = heightSize - 1;
    int area = 0;

    while (left < right) {
        area = max(area, (right - left) * min(height[left], height[right]));
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return area;
}
```

