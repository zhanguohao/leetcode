# LeetCode: 657. 判断路线成圈

[TOC]

## 1、题目描述





初始位置 (0, 0) 处有一个机器人。给出它的一系列动作，判断这个机器人的移动路线是否形成一个圆圈，换言之就是判断它是否会移回到**原来的位置**。

移动顺序由一个字符串表示。每一个动作都是由一个字符来表示的。机器人有效的动作有 `R`（右），`L`（左），`U`（上）和 `D`（下）。输出应为 true 或 false，表示机器人移动路线是否成圈。

**示例 1:**

```
输入: "UD"
输出: true
```

**示例 2:**

```
输入: "LL"
输出: false
```





## 2、题目解析

​	这个题的意思就是，是不是最终回到原点，感觉好傻的题，有种侮辱智商的感觉了





```c
bool judgeCircle(char* moves) {
    
    int x = 0;
    int y = 0;

    char *temp = moves;
    while (*temp) {
        switch (*temp) {
            case 'R':
                x++;
                break;
            case 'L':
                x--;
                break;
            case 'U':
                y++;
                break;
            case 'D':
                y--;
                break;
        }
        temp++;
    }

    return x == 0 && y == 0;
    
    
}
```

​	确认了一个问题，用if判断，比起switch，速度慢很多

。。。







```python
class Solution:
    def judgeCircle(self, moves):
        """
        :type moves: str
        :rtype: bool
        """
        x = 0
        y = 0

        for i in moves:
            if i == 'R':
                y += 1
            if i == 'L':
                y -= 1
            if i == 'U':
                x += 1
            if i == 'D':
                x -= 1

        if x == 0 and y == 0:
            return True

        return False
        
        
```

```python
class Solution:
    def judgeCircle(self, moves):
        """
        :type moves: str
        :rtype: bool
        """
        
        return moves.count('R') ==  moves.count('L') and moves.count('U') == moves.count('D')
```

