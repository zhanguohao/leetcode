# LeetCode: 232. 用栈实现队列

[TOC]



## 1、题目描述



使用栈实现队列的下列操作：

- push(x) -- 将一个元素放入队列的尾部。
- pop() -- 从队列首部移除元素。
- peek() -- 返回队列首部的元素。
- empty() -- 返回队列是否为空。

**示例:**

```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false
```

**说明:**

- 你只能使用标准的栈操作 -- 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
- 假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。





## 2、解题思路



​	没有直接写一个栈，然后模拟，这里主要写一下基本思路



- 1 首先，创建两个栈
- 2 然后现在想要入队一个元素，判断栈1，2都是空，入栈1
- 3继续入队，判断栈1不为空，入栈1；
- 4出队，判断栈1不为空，将栈1的每一个元素出栈，放到栈2中，最后将栈2的栈顶元素出栈，就可以，接着，将栈2中的所有元素弹出来，放入栈1
- 5 入队继续放到栈1
- 如果需要出队，继续执行第4步











```c
typedef struct {
    int *queue;
    int head;
    int rear;
    int size;
    int capacity;
} MyQueue;

/** Initialize your data structure here. */
MyQueue *myQueueCreate(int maxSize) {
    MyQueue *q = (MyQueue *) malloc(sizeof(MyQueue));
    q->queue = (int *) malloc(sizeof(int) * (maxSize));
    q->head = 0;
    q->rear = 0;
    q->size = 0;
    q->capacity = maxSize;
    
    return q;
}

/** Push element x to the back of queue. */
void myQueuePush(MyQueue *obj, int x) {
    if (obj->size < obj->capacity) {
        obj->queue[obj->rear] = x;
        obj->rear = (obj->rear + 1) % obj->capacity;
        obj->size++;
    }
}

/** Removes the element from in front of queue and returns that element. */
int myQueuePop(MyQueue *obj) {
    int temp = -1;
    if (obj->size > 0) {
        temp = obj->queue[obj->head];
        obj->head = (obj->head + 1) % obj->capacity;
        obj->size--;
    }

    return temp;
}

/** Get the front element. */
int myQueuePeek(MyQueue *obj) {
    return obj->size > 0 ? obj->queue[obj->head] : -1;


}

/** Returns whether the queue is empty. */
bool myQueueEmpty(MyQueue *obj) {
    return obj->size == 0;
}

void myQueueFree(MyQueue *obj) {
    if (obj) {
        if (obj->queue) {
            free(obj->queue);
        }
        free(obj);
    }
}




/**
 * Your MyQueue struct will be instantiated and called as such:
 * struct MyQueue* obj = myQueueCreate(maxSize);
 * myQueuePush(obj, x);
 * int param_2 = myQueuePop(obj);
 * int param_3 = myQueuePeek(obj);
 * bool param_4 = myQueueEmpty(obj);
 * myQueueFree(obj);
 */
```

