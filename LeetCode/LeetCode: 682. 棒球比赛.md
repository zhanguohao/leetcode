# LeetCode: 682. 棒球比赛

[TOC]



## 1、题目描述



你现在是棒球比赛记录员。
给定一个字符串列表，每个字符串可以是以下四种类型之一：
1.`整数`（一轮的得分）：直接表示您在本轮中获得的积分数。
2. `"+"`（一轮的得分）：表示本轮获得的得分是前两轮`有效` 回合得分的总和。
3. `"D"`（一轮的得分）：表示本轮获得的得分是前一轮`有效` 回合得分的两倍。
4. `"C"`（一个操作，这不是一个回合的分数）：表示您获得的最后一个`有效` 回合的分数是无效的，应该被移除。
每一轮的操作都是永久性的，可能会对前一轮和后一轮产生影响。
你需要返回你在所有回合中得分的总和。

**示例 1:**

```
输入: ["5","2","C","D","+"]
输出: 30
解释: 
第1轮：你可以得到5分。总和是：5。
第2轮：你可以得到2分。总和是：7。
操作1：第2轮的数据无效。总和是：5。
第3轮：你可以得到10分（第2轮的数据已被删除）。总数是：15。
第4轮：你可以得到5 + 10 = 15分。总数是：30。
```

**示例 2:**

```
输入: ["5","-2","4","C","D","9","+","+"]
输出: 27
解释: 
第1轮：你可以得到5分。总和是：5。
第2轮：你可以得到-2分。总数是：3。
第3轮：你可以得到4分。总和是：7。
操作1：第3轮的数据无效。总数是：3。
第4轮：你可以得到-4分（第三轮的数据已被删除）。总和是：-1。
第5轮：你可以得到9分。总数是：8。
第6轮：你可以得到-4 + 9 = 5分。总数是13。
第7轮：你可以得到9 + 5 = 14分。总数是27。
```

**注意：**

- 输入列表的大小将介于1和1000之间。
- 列表中的每个整数都将介于-30000和30000之间。





## 2、解题思路



```c
int sToNum(char *s) {

    int result = 0;
    int left = 0;
    int sign = 1;
    if (s[0] == '-') {
        sign = -1;
        left = 1;
    }

    for (int i = left; i < strlen(s); i++) {
        result = result * 10 + s[i] - '0';
    }

    return result * sign;

}

int calPoints(char** ops, int opsSize) {
    // 标识无效数据
    const int MIN = -1000000;
    int *result = (int *) malloc(sizeof(int) * opsSize);


    // 初始化
    for (int i = 0; i < opsSize; i++) {
        result[i] = MIN;
    }

    int first = MIN;
    int second = MIN;

    int j;
    for (int i = 0; i < opsSize; i++) {

        switch (ops[i][0]) {
            case 'C':
                j = i;
                while (result[j] == MIN) {
                    j--;
                }
                if (j >= 0) {
                    result[j] = MIN;
                }

                break;
            case 'D':
                j = i;
                while (result[j] == MIN) {
                    j--;
                }
                if (j >= 0) {
                    result[i] = 2 * result[j];
                }
                break;
            case '+':
                first = MIN;
                second = MIN;

                j = i;
                while (result[j] == MIN) {
                    j--;
                }
                first = result[j];
                j--;
                while (result[j] == MIN) {
                    j--;
                }
                second = result[j];
                result[i] = first + second;

                break;
            default:
                result[i] = sToNum(ops[i]);
                break;
        }


    }

    int ans = 0;

    
    
    
    for (int i = 0; i < opsSize; i++) {
        if (result[i] != MIN) {
            ans += result[i];
        }
        printf("%d\n",result[i]);
    }
    return ans;
}
```

