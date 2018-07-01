# LeetCode: 696. 计数二进制子串

[TOC]



## 1、题目描述



给定一个字符串 `s`，计算具有相同数量0和1的非空(连续)子字符串的数量，并且这些子字符串中的所有0和所有1都是组合在一起的。

重复出现的子串要计算它们出现的次数。

**示例 1 :**

```
输入: "00110011"
输出: 6
解释: 有6个子串具有相同数量的连续1和0：“0011”，“01”，“1100”，“10”，“0011” 和 “01”。

请注意，一些重复出现的子串要计算它们出现的次数。

另外，“00110011”不是有效的子串，因为所有的0（和1）没有组合在一起。
```

**示例 2 :**

```
输入: "10101"
输出: 4
解释: 有4个子串：“10”，“01”，“10”，“01”，它们具有相同数量的连续1和0。
```

**注意：**

- `s.length` 在1到50,000之间。
- `s` 只包含“0”或“1”字符。



## 2、解题思路

### 2.1 遍历法

​	直接能想到的就是遍历法，不过因为时间超过了限制，所以要寻找其他办法

```c

bool isSubstring(char *s) {
    int current_count = 0;
    int next_count = 0;

    char cur = *s;
    char *temp = s;
    bool current_char = true;
    while (*temp) {

        if (current_char) {

            if (*temp == cur) {
                current_count++;
            } else {
                current_char = false;
                next_count++;
                if (next_count >= current_count) {
                    return true;
                }
            }
        } else {
            if (*temp != cur) {
                next_count++;
                if (next_count >= current_count) {
                    return true;
                }
            } else {
                break;
            }
        }
        temp++;
    }


    return current_count <= next_count;
}


int countBinarySubstrings(char *s) {
    int result = 0;
    char *temp = s;
    while (*temp) {
        result += isSubstring(temp);
        temp++;
    }

    return result;

}


```



### 2.2 相邻最小值法

​	实际上，如果相邻的值中，我们只取最小值，

例如

00011

统计数字是3，2，子串是0011，01，也就是说，子串的数量等于最小的那个值得数量；

​	重要的是理解题意！！！！



```c
#define min(a, b) a<b?a:b;
int countBinarySubstrings(char* s) {
    int prev_count = 0;
    int current_count = 0;

    int result = 0;

    char *temp = s;
    char cur_char = *temp;
    while (*temp) {

        if (*temp == cur_char) {
            current_count++;
        } else {
            cur_char = *temp;
            result += min(prev_count, current_count);
            prev_count = current_count;
            current_count = 1;
        }

        temp++;

    }

    result += min(current_count, prev_count);

    return result;
}
```

