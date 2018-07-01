# LeetCode: 155. 最小栈

[TOC]

## 1、题目描述



设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

- push(x) -- 将元素 x 推入栈中。
- pop() -- 删除栈顶的元素。
- top() -- 获取栈顶元素。
- getMin() -- 检索栈中的最小元素。

**示例:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```



## 2、解题思路

### 2.1 关联数值法

​	正常情况，能够想到的就是直接将值存放到变量中，这样得到的每一次能够直接取得最小元素

​	但是，查询最小元素的时间却是$O(n)$

​	

​	考虑到进栈，出栈的情况，如果栈中每一个只都能够跟最小值关联，也就是能够直接计算处来最小值，就可以直接得到最小值

​	例如，正常的想法，我们将每一个数直接入栈，但是我们现在想要每一个值与最小值进行关联，怎么做呢，取差值，例如，当前的最小值是2，入栈的值是5，5-2=3，我们将3存放到栈中，如果想要得到栈顶元素，就用最小值加上3就能得到



实例

​	入栈顺序是 2，1，3，-1，5

第一个元素

​	最小值是2，栈顶值为0

第二个元素

​	栈顶值为 1- 2 = -1，因为小于0，表示新值小于最小值，需要更新最小值，最小值为当前的元素值，1

第三个元素

​	栈顶值为 3-1 = 2 ，大于零，表示最小值不用更新

第四个元素

​	栈顶值为	-1-1 = -2， 小于0，表示需要更新最小值，最小值为-1

第五个元素

​	栈顶值为5 - （-1） = 6，大于零，不用更新最小值



因此，栈中的元素值为

```
0	-1	2	-2	6
最小值为：
2	1	1	-1	-1
```



当我们在push的时候，就根据上面的规则

如果需要pop的时候，需要判断这个值是大于零还是小于零，如果是大于零，表示不需要更新最小值，如果是小于零，就需要更新最小值



出栈

第五个元素

​	栈顶值大于零，表示不需要更新最小值，入股需要返回值，就用最小值计算

​	6 +（-1） = 5

第4个元素

​	栈顶值小于零，表示此处需要更新最小值，这时候，当前的最小值就是这个点的值

​	计算最小值

​	-（-2）+ -1 = 1， 最小值更新为1



其他的以此类推



==注意:  这个方法存在问题，会有数值计算的时候的问题，会超过int的限制==





​	下面的程序是有一点bug的，问题出在边界条件上

​	如果当前最小值是2147483648, push 一个- 2147483648， 这时候，本来应该更新最小值的，但是，因为计算的限制，-2147483648 - 2147483648 = 1， 这样一来，就无法得到正确的结果了



```c
#include <stdlib.h>
#include <stdbool.h>
#include "stdio.h"
#include "string.h"

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */



typedef struct {
    int size;
    int *stack;
    int min_value;
    int top_pos;
} MinStack;

/** initialize your data structure here. */
MinStack *minStackCreate(int maxSize) {
    MinStack *minStack = (MinStack *) malloc(sizeof(MinStack));
    minStack->min_value = INT32_MIN;
    minStack->top_pos = -1;
    minStack->stack = (int *) malloc(sizeof(int) * maxSize);
    minStack->size = maxSize;

    return minStack;
}

void minStackPush(MinStack *obj, int x) {
    if (!obj) {
        return;
    }
    // 初始化
    if (obj->top_pos == -1) {
        obj->min_value = x;
        obj->stack[++obj->top_pos] = 0;
    } else if (obj->top_pos < obj->size - 1) {
        obj->stack[++obj->top_pos] = x - obj->min_value;
        // 如果小于零，表示需要更新最小值
        if (obj->stack[obj->top_pos] < 0) {
            obj->min_value = x;
        }
    }

}

void minStackPop(MinStack *obj) {
    if (!obj || obj->top_pos <= -1) {
        return;
    }

    // 判断栈顶的值
    if (obj->stack[obj->top_pos] < 0) {
        obj->min_value = obj->min_value - obj->stack[obj->top_pos];
        obj->top_pos--;
    } else {
        obj->top_pos--;
    }

    if (obj->top_pos <= -1) {
        obj->min_value = INT32_MIN;
    }
}

int minStackTop(MinStack *obj) {
    if (!obj || obj->top_pos <= -1) {
        return INT32_MIN;
    }
    // 判断栈顶的值是不是小于零，是的话，就返回最小值即可
    if (obj->stack[obj->top_pos] < 0) {
        return obj->min_value;
    } else {
        return obj->stack[obj->top_pos] + obj->min_value;
    }

}

int minStackGetMin(MinStack *obj) {
    if (!obj || obj->top_pos <= -1) {
        return INT32_MIN;
    }

    return obj->min_value;

}

void minStackFree(MinStack *obj) {
    if (obj) {
        if (obj->stack) {

            free(obj->stack);
        }
        free(obj);
    }
}

/**
 * Your MinStack struct will be instantiated and called as such:
 * struct MinStack* obj = minStackCreate(maxSize);
 * minStackPush(obj, x);
 * minStackPop(obj);
 * int param_3 = minStackTop(obj);
 * int param_4 = minStackGetMin(obj);
 * minStackFree(obj);
 */

void pprint(MinStack *s) {
    printf("=====================\n");
    for (int i = 0; i < 4; i++) {
        printf("%d\t", s->stack[i]);
    }
    printf("\nmin value : %d\n", s->min_value);
    printf("=====================\n");
}

int main() {

    MinStack *s = minStackCreate(10);
    minStackPush(s, 2);
    minStackPush(s, 0);
    minStackPush(s, 3);
    minStackPush(s, 0);
    pprint(s);
    printf("%d\n", minStackGetMin(s));
    minStackPop(s);
    printf("%d\n", minStackGetMin(s));
    minStackPop(s);
    printf("%d\n", minStackGetMin(s));
    minStackPop(s);
    printf("%d\n", minStackGetMin(s));


    printf("==========\n");
    printf("%d\n", INT32_MIN - INT32_MAX);

}
```





### 2.2 备用数组法

​	考虑到上面的问题，因此，使用一个数组专门存储最小值

```c
typedef struct {
    int size;
    int *stack;
    int *min_value;
    int top_pos;
} MinStack;

/** initialize your data structure here. */
MinStack *minStackCreate(int maxSize) {
    MinStack *minStack = (MinStack *) malloc(sizeof(MinStack) * maxSize);
    minStack->top_pos = -1;
    minStack->stack = (int *) malloc(sizeof(int) * maxSize);
    minStack->min_value = (int *) malloc(sizeof(int) * maxSize);;
    minStack->size = maxSize;
    return minStack;
}

void minStackPush(MinStack *obj, int x) {
    if (!obj) {
        return;
    }
    // 初始化
    if (obj->top_pos == -1) {
        obj->top_pos++;
        obj->min_value[obj->top_pos] = x;
        obj->stack[obj->top_pos] = x;
    } else if (obj->top_pos < obj->size - 1) {
        obj->stack[++obj->top_pos] = x;

        if (obj->min_value[obj->top_pos - 1] > x) {
            obj->min_value[obj->top_pos] = x;
        } else {
            obj->min_value[obj->top_pos] = obj->min_value[obj->top_pos - 1];
        }

    }

}

void minStackPop(MinStack *obj) {
    if (!obj || obj->top_pos <= -1) {
        return;
    }

    // 栈顶减一
    
    obj->top_pos--;
}

int minStackTop(MinStack *obj) {
    if (!obj || obj->top_pos <= -1) {
        return INT32_MIN;
    }
    // 返回栈顶的值
    return obj->stack[obj->top_pos];

}

int minStackGetMin(MinStack *obj) {
    if (!obj || obj->top_pos <= -1) {
        return INT32_MIN;
    }

    return obj->min_value[obj->top_pos];

}

void minStackFree(MinStack *obj) {
    if (obj) {
        if (obj->stack) {
            free(obj->stack);
        }
        if (obj->min_value) {
            free(obj->min_value);
        }
        free(obj);
    }
}


// MinStack* minStackCreate(int maxSize) {
    
// }

// void minStackPush(MinStack* obj, int x) {
    
// }

// void minStackPop(MinStack* obj) {
    
// }

// int minStackTop(MinStack* obj) {
    
// }

// int minStackGetMin(MinStack* obj) {
    
// }

// void minStackFree(MinStack* obj) {
    
// }

/**
 * Your MinStack struct will be instantiated and called as such:
 * struct MinStack* obj = minStackCreate(maxSize);
 * minStackPush(obj, x);
 * minStackPop(obj);
 * int param_3 = minStackTop(obj);
 * int param_4 = minStackGetMin(obj);
 * minStackFree(obj);
 */
```

