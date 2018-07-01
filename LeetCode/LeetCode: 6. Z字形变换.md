# LeetCode: 6. Z字形变换

[TOC]

```
ZigZag变换，是一种打乱顺序的方式
```





## 1、题目要求

```
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

P   A   H   N
A P L S I I G
Y   I   R
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);
Example 1:

Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
Example 2:

Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```



将字符串 `"PAYPALISHIRING"` 以Z字形排列成给定的行数：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后从左往右，逐行读取字符：`"PAHNAPLSIIGYIR"`

实现一个将字符串进行指定行数变换的函数:

```
string convert(string s, int numRows);
```

**示例 1:**

```
输入: s = "PAYPALISHIRING", numRows = 3
输出: "PAHNAPLSIIGYIR"
```

**示例 2:**

```
输入: s = "PAYPALISHIRING", numRows = 4
输出: "PINALSIGYAHRPI"
解释:

P     I    N
A   L S  I G
Y A   H R
P     I
```



## 2、解题思路

### 2.1 依次写入法

​	如果有5行，则准备5个缓冲，将不同的字符依次放入不同的缓冲，然后从第一个开始，依次取出即可得到

### 2.2 移位写入法

​	

| 行号 |      |      |      |      |      |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1    | 0    |      | 4    |      | 8    |
| 2    | 1    | 3    | 5    | 7    | 9    |
| 3    | 2    |      | 6    |      | 10   |

​	根据上面的写法，数据第一行的，下标间隔为4，第2行讲个为2，第三行间隔为4

| 行   |      |      |      |      |      |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1    | 0    |      |      |      | 8    |
| 2    | 1    |      |      | 7    | 9    |
| 3    | 2    |      | 6    |      | 10   |
| 4    | 3    | 5    |      |      | 11   |
| 5    | 4    |      |      |      | 12   |

​	再根据上面的表格，

第一行间隔为：8

第二行间隔为：6，2

第三行间隔为：4，4

第四行间隔为：2，6

第五行间隔为：8



​	根据上面的规律，可以看到，如果每一行都看成是两个间隔，就能统一，除去第一行与最后一行，如下改一下，就可以得到每一次移动的下标数；

第一行间隔为：8，8

第二行间隔为：6，2

第三行间隔为：4，4

第四行间隔为：2，6

第五行间隔为：8，8





```c
char *convert(char *s, int numRows) {

    int i = 0, moves[2];
    int result_pos = 0;
    int source_pos;
    int length = 0;
    int count = 0;
    while (s[i++]) {
        length++;
    }
    i = 0;
    char *result = (char *) malloc(sizeof(char) * length + 1);
    if (numRows <= 1) {
        while (i <= length) {
            result[i] = s[i];
            i++;
        }
        return result;
    }
    for (i = 0; i < numRows; i++) {
        if (i == 0 || i == (numRows - 1)) {
            moves[0] = moves[1] = (numRows - 1) * 2;
        } else {
            moves[0] = (numRows - 1) * 2 - 2 * i;
            moves[1] = 2 * i;
        }
        source_pos = i;
        count = 0;
        while (source_pos < length) {
            result[result_pos++] = s[source_pos];
            source_pos += moves[(count++) % 2];
        }
    }
    result[length] = '\0';
    return result;
}
```