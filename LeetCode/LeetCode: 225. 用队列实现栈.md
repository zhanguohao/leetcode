# LeetCode: 225. 用队列实现栈

[TOC]







## 1、题目描述



使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

**注意:**

- 你只能使用队列的基本操作-- 也就是 `push to back`, `peek/pop from front`, `size`, 和 `is empty` 这些操作是合法的。
- 你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
- 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。



## 2、解题思路



​	因为在C的库里面，没有已经实现的队列，这里主要是写一下实现思路



​	想要用队列模拟一个栈，首先我们看到，队列是先入先出，栈是后入先出，先要模拟栈的操作，我们需要使用两个队列

​	假设现在有两个队列，队列1，队列2，想在想要入栈，

​	判断队列1，2为空，将元素放入队列1中，

​	继续入栈，队列1不为空，将元素放到队列1中



​	然后想要出栈，该如何做呢，首先将除了最后一个元素，其他所有的都出队列，放到另一个队列中，然后本队列就只剩下一个元素了，将这个元素弹出，并返回

​	继续想要入栈，这时候，队列2不为空，放到队列2里面

​	入栈，放到队列2中

​	出栈，将队列2中除了最后一个元素其他都出队列，放到第一个队列里面，然后将最后一个返回

























```c
typedef struct {
    int *stack;
    int size;
    int head;
} MyStack;

/** Initialize your data structure here. */
MyStack *myStackCreate(int maxSize) {
    MyStack *s = (MyStack *) malloc(sizeof(MyStack));
    s->stack = (int *) malloc(sizeof(int) * maxSize);
    s->size = maxSize;
    s->head = -1;
    return s;
}

/** Push element x onto stack. */
void myStackPush(MyStack *obj, int x) {
    if (obj->head < obj->size - 1) {
        obj->stack[++obj->head] = x;
    }
}

/** Removes the element on top of the stack and returns that element. */
int myStackPop(MyStack *obj) {
    if (obj->head >= 0) {
        return obj->stack[obj->head--];
    } else {
        return -1;
    }
}

/** Get the top element. */
int myStackTop(MyStack *obj) {
    if (obj->head >= 0) {
        return obj->stack[obj->head];
    } else {
        return -1;
    }
}

/** Returns whether the stack is empty. */
bool myStackEmpty(MyStack *obj) {
    if (obj->head <= -1) {
        return true;
    } else {
        return false;
    }
}

void myStackFree(MyStack *obj) {
    if (obj) {
        if (obj->stack) {
            free(obj->stack);
        }
        free(obj);
    }
}





/**
 * Your MyStack struct will be instantiated and called as such:
 * struct MyStack* obj = myStackCreate(maxSize);
 * myStackPush(obj, x);
 * int param_2 = myStackPop(obj);
 * int param_3 = myStackTop(obj);
 * bool param_4 = myStackEmpty(obj);
 * myStackFree(obj);
 */
```

