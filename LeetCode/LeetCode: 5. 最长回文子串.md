# LeetCode: 5. 最长回文子串

[TOC]

```
最长回文子串

	回文串（palindromic string）是指这个字符串无论从左读还是从右读，所读的顺序是一样的；简而言之，回文串是左右对称的。所谓最长回文子串问题，是指对于一个给定的母串，找到的一个子串，是回文串，并且长度最长
```



## 1、题目描述

```

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example 1:

Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
Example 2:

Input: "cbbd"
Output: "bb"
```



给定一个字符串 **s**，找到 **s** 中最长的回文子串。你可以假设 **s** 的最大长度为1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba"也是一个有效答案。
```

**示例 2：**

```
输入: "cbbd"
输出: "bb"
```



## 2、解题思路

### 2.1 穷举法

​	从字符串的左侧，依次开始扫描，使用双重循环，将所有的子串得到，然后判断子串是否是回文串，如果是，判断长度是否是最长的，是最长的，就更新长度，并记录这个字串

​	不过需要分奇偶进行判断

### 2.2 动态规划

​	动态规划的思路就是，利用前面的结果进行判断，例如
$$
c[i,j] = \left\{ {\matrix{
   c[i+1, j-1] & {if \ s[i] = s[j]}  \cr 
   0 & {if \ s[i] \ne s[j]}  \cr 
 } } \right.
$$
​	如上所示，i和j表示下标，i在前，j在后，因此，判断的时候，如果判断到当前的是否是回文串，可以利用前面的结果来判断是不是回文串，比起前面构造的穷举法，少了一层循环

### 2.3 分治法

​	考虑回文串中间部分是相同的，从母串的每一个字符入手，向左右判断回文串的长度

### 2.4 Manacher

​	这个算法主要思想集中在将奇偶数的判断放在一起

​	例如下面的字符串

```
abcde
变成
#a#b#c#d#e#
长度从5变成了11，也就是 2*n+1

如果是ab
#a#b#
长度从2变成了5，同样是2*n +1

也就是说，不管是奇数个还是偶数个字母，变换完成以后，都是奇数个
```

​	然后进行分析，如果当前判断的所在的下标被前面的回文串的最右侧下标所包含，，根据回文串的对称性，当前下标相对于前面的id有一个对称点，这个点的回文串长度我们已经算出来了，跟当前点到maxid的差值作比较，取小的，然后在这个基础上继续判断回文串的长度，也就是利用了前面应计算的结果，降低了计算量



​	这个算法的关键点，就在于，如果当前要判断的点在前面的点中的回文串中，那么肯定能够知道，这个点对称的位置的回文串长度，也能知道到最右端回文串的距离，这时候，就饿能根据这两个值，跳过一些已经验证过的步骤

```
************$***1***i***j***$--------------
假如我们判断到了j这个点，1是与j通过i对称的点，1的回文串半径已经确定了，那么j的回文串半径在右面$之前的部分也就能确定了，因为是对称的嘛
	所以，基本的思路就是这样，跳过已经判断好的一些点
```



```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

char *longestPalindrome(char *s) {
    int i=0,id,maxid,ans=1,pos=0;
    //找到的最长回文串的左端下标
    int left;
    //最长回文串的临时数组
    char * result;
    //字符串长度
    int str_length=0;
    while(s[i++]) str_length++;

    //初始化新的数组,存放每个元素对应的回文半径
    int p[2*str_length+3];
    //构造新的字符数组，用来计算
    char new_str[2*str_length+3];
    new_str[0]='$';
    new_str[1]='#';
    for (i =0;i< str_length;i++){
        new_str[2*i+2]=s[i];
        new_str[2*i+3]='#';
    }
    new_str[2*str_length+2]='\0';

    int len = 2*str_length+2;

    // 对每个字符求其回文半径，利用前面的数组
    // 最后面的'\0'不用求
    // 第一个字符"$"，p[0]=1
    id = 0;
    maxid = 1;
    p[0] = 1;
    for (i=1;i<len;i++){
        // 判断前一次的回文最右端是不是包含当前判断的字符
        if (maxid > i){
            p[i] = p[2*id-i] > (maxid-i)?(maxid-i):p[2*id-i];
        }else{
            p[i] = 1;
        }
        while(new_str[i+p[i]] == new_str[i-p[i]]){
            p[i]++;
        }
        if(p[i]+i> maxid){
            maxid = p[i]+i;
            id = i;
        }
        if (ans<p[i]){
            ans = p[i];
            pos = i;
        }
    }


    left = (pos-ans)/2;
    result = (char*)malloc(sizeof(char)*ans);
    for (i=0;i<ans-1;i++){
        result[i] = s[left+i];
    }
    result[ans-1] = '\0';

    return result;

}
int main() {
    char * aa= "abb";
    printf("%s\n", longestPalindrome(aa));
}
```



输出：

```
/Users/zhangguohao/CLionProjects/untitled/cmake-build-debug/untitled
bb

Process finished with exit code 0
```

