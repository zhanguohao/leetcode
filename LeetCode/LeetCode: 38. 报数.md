# LeetCode: 38. 报数

[TOC]



## 1、题目描述

```
报数序列是指一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：

1.     1
2.     11
3.     21
4.     1211
5.     111221
1 被读作  "one 1"  ("一个一") , 即 11。
11 被读作 "two 1s" ("两个一"）, 即 21。
21 被读作 "one 2",  "one 1" （"一个二" ,  "一个一") , 即 1211。

给定一个正整数 n ，输出报数序列的第 n 项。

注意：整数顺序将表示为一个字符串。

示例 1:

输入: 1
输出: "1"
示例 2:

输入: 4
输出: "1211"
```

## 2、解题思路

​	一开始，看了半天没明白什么意思，后来才明白

​	这个序列是从1开始的，然后我们读出来，也就是one 1，变成了11，也就是说，下一个数是上一个数读音变化出来的

​	11，读成"two 1s"， 变成了21

以此类推：

1

11

21

1211

111221

21112211

1221112221



。。。



​	明确了题目的意思，也就容易写了

- 如果当前是1 并且下一个不是1，就直接变成11
- 如果当前是1，并且下一个是1，变成21
- 如果当前是2，下一个不是2，变成12
- 如果当前是2，下一个是2，变成22

只会出现这4种情况

​	如果用一个数组实现，发现是有点问题的，需要不断地移动数据

​	因此，选择使用两个数组，一个存放上一次的结果，另一个存放根据上一次计算出来的结果

 	需要非常注意的就是，这两个数组的长度问题

​	考虑到最大的可能性，新的数组长度可能是上一个的2倍	

```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <memory.h>
#include <string.h>

char *countAndSay(int n) {
    if (n == 0) {
        return "";
    }
    char *result = (char *) malloc(sizeof(char) * 2);
    char *next = (char *) malloc(strlen(result) * 2);
    char *temp;

    result[0] = '1';
    result[1] = '\0';
    int pos = 0;
    int next_pos = 0;
    int equal_num = 1;
    for (int i = 1; i < n; i++) {
        next_pos = 0;
        pos = 0;
        while (result[pos]) {
            // 判断有几个相同字符
            // 如果下一个字符存在，一直判断有几个相同的字符
            if (result[pos + 1] == result[pos]) {

                while (result[pos] == result[pos + 1]) {
                    equal_num++;
                    pos++;
                }
                pos++;
                next[next_pos] = '0' + equal_num;
                next[next_pos + 1] = result[pos - equal_num];
                next_pos += 2;
                equal_num = 1;
            }

                // 如果下一个字符是'\0'
            else if (!result[pos + 1]) {
                next[next_pos] = '1';
                next[next_pos + 1] = result[pos];
                next[next_pos + 2] = '\0';
                pos++;
                next_pos += 2;
            }
                // 如果下一个字符与当前字符不相同
            else if (result[pos + 1] && result[pos] != result[pos + 1]) {
                next[next_pos] = '1';
                next[next_pos + 1] = result[pos];;
                pos++;
                next_pos += 2;
            }

        }
        next[next_pos] = '\0';
        temp = result;
        result = next;
        free(temp);
        next = (char *) malloc(strlen(result) * 2);
    }

    free(next);
    int count = 0;
    {
        while (result[count]) {
            count++;
        }
    }
    printf("number: %d \n", n);
    printf("length: %d \n", count);
    return result;

}

void print(int x) {
    int num = 1;
    for (int i = 31; i >= 0; i--) {
        if (x & (num << i)) {
            printf("1");
        } else {
            printf("0");
        }
    }
    printf("\n");
}

int main() {


    printf("%s\n", countAndSay(1));
    printf("%s\n", countAndSay(2));
    printf("%s\n", countAndSay(3));
    printf("%s\n", countAndSay(4));
    printf("%s\n", countAndSay(5));
    printf("%s\n", countAndSay(6));
    printf("%s\n", countAndSay(7));
    printf("%s\n", countAndSay(8));
    printf("%s\n", countAndSay(9));
    printf("%s\n", countAndSay(10));
    printf("%s\n", countAndSay(11));
    printf("%s\n", countAndSay(12));
    printf("%s\n", countAndSay(13));
    printf("%s\n", countAndSay(14));
    printf("%s\n", countAndSay(25));
    printf("%s\n", countAndSay(30));
    printf("%s\n", countAndSay(50));


}

```

```
/Users/zhangguohao/CLionProjects/untitled/cmake-build-debug/untitled
number: 1 
length: 1 
1
number: 2 
length: 2 
11
number: 3 
length: 2 
21
number: 4 
length: 4 
1211
number: 5 
length: 6 
111221
number: 6 
length: 6 
312211
number: 7 
length: 8 
13112221
number: 8 
length: 10 
1113213211
number: 9 
length: 14 
31131211131221
number: 10 
length: 20 
13211311123113112211
number: 11 
length: 26 
11131221133112132113212221
number: 12 
length: 34 
3113112221232112111312211312113211
number: 13 
length: 46 
1321132132111213122112311311222113111221131221
number: 14 
length: 62 
11131221131211131231121113112221121321132132211331222113112211
number: 25 
length: 1182 
132113213221133112132123123112111311222112132113311213211231232112311311222112111312211311123113322112132113212231121113112221121321132132211231232112311321322112311311222113111231133221121113122113121113221112131221123113111231121123222112132113213221133112132123123112111312111312212231131122211311123113322112111312211312111322111213122112311311123112112322211211131221131211132221232112111312111213111213211231132132211211131221232112111312212221121123222112132113213221133112132123123112111311222112132113213221132213211321322112311311222113311213212322211211131221131211221321123113213221121113122113121132211332113221122112133221123113112221131112311332111213122112311311123112111331121113122112132113121113222112311311221112131221123113112221121113311211131122211211131221131211132221121321132132212321121113121112133221123113112221131112212211131221121321131211132221123113112221131112311332211211133112111311222112111312211311123113322112111312211312111322212321121113121112133221121321132132211331121321231231121113112221121321132122311211131122211211131221131211322113322112111312211322132113213221123113112221131112311311121321122112132231121113122113322113111221131221
number: 30 
length: 4462 
..
 
```

​	后面的数据太长。。。



