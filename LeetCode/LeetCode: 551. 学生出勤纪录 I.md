# LeetCode: 551. 学生出勤纪录 I

[TOC]



## 1、题目描述



给定一个字符串来代表一个学生的出勤纪录，这个纪录仅包含以下三个字符：

1. **'A'** : Absent，缺勤
2. **'L'** : Late，迟到
3. **'P'** : Present，到场

如果一个学生的出勤纪录中不**超过一个'A'(缺勤)**并且**不超过两个连续的'L'(迟到)**,那么这个学生会被奖赏。

你需要根据这个学生的出勤纪录判断他是否会被奖赏。

**示例 1:**

```
输入: "PPALLP"
输出: True
```

**示例 2:**

```
输入: "PPALLL"
输出: False
```





## 2、解题思路

```c
bool checkRecord(char* s) {
    
    int A_count = 0;
    int L_count = 0;

    bool start_Ls = false;

    for (int i = 0; i < strlen(s); i++) {
        if (A_count >= 2 || L_count >= 3) {
            return false;
        }


        if (s[i] == 'A') {
            A_count++;
        }
        if (!start_Ls) {
            if (s[i] == 'L') {
                start_Ls = true;
                L_count++;
            }
        } else {
            if (s[i] == 'L') {
                L_count++;
            } else {
                start_Ls = false;
                L_count = 0;
            }
        }
    }


    if (A_count >= 2 || L_count >= 3) {
        return false;
    }

    return true;
    
}
```

