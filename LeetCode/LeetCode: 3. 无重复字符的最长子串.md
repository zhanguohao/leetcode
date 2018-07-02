# LeetCode: 3. 无重复字符的最长子串

[TOC]

```
最长不重复字符子串
```



## 1、题目描述

```
Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

​	给定一个字符串，找出不含有重复字符的**最长子串**的长度。

**示例：**

给定 `"abcabcbb"` ，没有重复字符的最长子串是 `"abc"` ，那么长度就是3。

给定 `"bbbbb"` ，最长的子串就是 `"b"` ，长度是1。

给定 `"pwwkew"` ，最长子串是 `"wke"` ，长度是3。请注意答案必须是一个**子串**，`"pwke"` 是 *子序列*  而不是子串。



## 2、解题思路

### 2.1 穷举法

​	双重循环得到所有的子串，然后判断每个是不是不重复的，是的话，更新最大不重复子串的值

### 2.2 滑动窗口法

​	设计两个指针，i，j指向字符串的下标；

​	（j-i表示不重复子串的值）

​	从前向后遍历，遇到一个字符，判断这个字符是不是在集合中，如果不在，就将这个字符放到集合中；

​	如果在，就将第i个字符从集合中删除，i加一，一直删除到前一个a为止

​	例如下面的字符串：

​	abcad

- 初始化，i=0，j=0
- j不断递增，当j=3，发现集合中有重复的字符串了，然后开始删除，将a删除，然后i指向1
- 将a放入集合中
- 不断地重复操作



### 2.3 哈希表法 

​	在集合中，我们需要不断地删除一个元素，才能够移动i指针，如果能够直接找到新的指针，就能够直接更新i的值，采用key-value的方式，将字符看成key，下标看成value

### 2.4 缓存表

​	哈希表的实现较为复杂，如果已知字符的个数，字符在ASCII表中的位置，创建一张表，存储当前字符的下标，不断地更新；

```c
int lengthOfLongestSubstring(char* s) {
    int index[128],i,j;
    int ans = 0;
    for (i=0;i<128;i++){
        index[i] =0 ;
    }
    for(i=0,j=0;s[j];j++){
        i = i > index[s[j]]? i : index[s[j]];
        ans = ans > j-i+1 ? ans : j-i+1;
        index[s[j]] = j + 1;
    }
    return ans;
}
```

