# LeetCode: 383. 赎金信

[TOC]



## 1、题目描述



给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串ransom能不能由第二个字符串magazines里面的字符构成。如果可以构成，返回 true ；否则返回 false。

(题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。)

**注意：**

你可以假设两个字符串均只含有小写字母。

```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```





## 2、解题思路



```c
bool canConstruct(char* ransomNote, char* magazine) {
    int ran_count[26];
    int mag_count[26];

    for (int i = 0; i < 26; i++) {
        ran_count[i] = 0;
        mag_count[i] = 0;
    }


    int length_ran = strlen(ransomNote);
    int length_mag = strlen(magazine);

    for (int i = 0; i < length_ran; i++) {
        ran_count[ransomNote[i] - 'a']++;
    }

    for (int i = 0; i < length_mag; i++) {
        mag_count[magazine[i] - 'a']++;
    }
    
    
    for(int i =0;i<26;i++){
        if(ran_count[i] > mag_count[i]){
            return false;
        }
    }

    return true;
}
```



