---
title: '从零开始学数据结构之队列（一）'
date: 2019-11-17 17:16:04
tags: [数据结构]
published: true
hideInList: false
feature: https://tva2.sinaimg.cn/large/0072Vf1pgy1foxkfbb3c7j31kw0w04ou.jpg
---

队列也是一种操作受限制的线性表，它拥有队首与队尾，并规定只能从队首删除元素（出队），从队尾添加元素（入队），队列的特点就是先进先出(First In First Out)。
<!-- more -->

1. 首先是队列的顺序存储结构，其数据结构如下
```c++
        #define MaxSize 50
        typedef int ElemType;
        typedef struct
        {
            ElemType data[MaxSize];
            int front, // 队头 允许出队
                rear;  // 队尾 允许入队
        } SqQueue;
```
2. 其次是队列的常用操作：

> 同栈的操作一样，先判断一下队列的一些临界状态：
- 队列为空：则可以通过判断队列的首尾指针来确定，即q.front == q.rear，此时不允许出队。
- 队列已满：则通过判断队尾指针等于初始分配的最大空间，即q.rear == MaxSize（真的可以吗？🤔），此时不允许入队。


具体代码实现如下：

1. 队列初始化

```c++
            void InitQueue(SqQueue &q)
                {
                    q.front = q.rear = 0;
                }
```

2. 判断队列是否为空

```c++
            bool QueueEmpty(SqQueue &q)
            {
                if (q.front == q.rear)
                {
                    return true;
                }
                return false;
            }
```

3. 入队

```c++
            bool EnQueue(SqQueue &q, ElemType e)
                {
                    if (q.rear == MaxSize)
                    {
                        return false;
                    }
                    q.data[q.rear++] = e;
                    return true;
                }
```

4. 出队
```c++
            bool DeQueue(SqQueue &q, ElemType &e)
            {
                if (q.front == q.rear)
                {
                    return false;
                }
                e = q.data[q.front--];
                return true;
            }
```



5. 读取队头元素

```c++
            bool GetHead(SqQueue &q, ElemType &e)
                {
                    bool flag = QueueEmpty(q);
                    if (flag)
                    {
                        return false;
                    }
                    e = q.data[q.front];
                    return true;
                }
```

以上看似已经完成了队列的顺序存储，但此时又一个严重的问题就是，当队尾指针q.rear == MaxSize时，再执行出队操作，出队若干元素之后，再执行入队操作时会出现“上溢出”，明明数组data还有闲置的空间，这是一种“假溢出”的现象。那么现在开始，把上面那个data数组想象成一个圆环，将数组的首尾相接形成一个封闭的回路，那么就得到一个循环队列。在循环队列中解决这个问题就方便很多了，有很多的方法🤗，比如说
1. 在队列中加入一个表示当前存储的元素长度的字段size，
2. 或者加入一个记录当前操作的字段flag。当flag为1时表示最后一次执行的时出队操作，此时判断q.front == q.rear，则为队空状态；为0时表示最后一次执行的是入队操作，此时判断q.front == q.rear，则为队满的状态。
3. 还有一种方法就是牺牲一个存储空间，当q.front == q.rear+1时，就表示队列已满，队列为空时则仍是q.front == q.rear；不过之前临界的判断规则需要修改一下😹：
  - 队列已满：q.front = (q.rear+1)%MaxSize;
  - 队列元素的个数：count = (q.rear-q.front+MaxSize)%MaxSize;

第三种方法的入队、出队具体代码实现如下：

```c++
    // 入队

    bool EnQueue(SqQueue &q, ElemType e)
    {
        if ((q.rear+1)%MaxSize == q.front) // 此时表示队列已满
        {
            return false;
        }
        q.data[q.rear] = e;
        q.rear = (q.rear+1)%MaxSize;
        return false;
    }

    // 出队

    bool DeQueue(SqQueue &q, ElemType &e)
    {
        if (q.front == q.rear)
        {
            return false;
        }
        e = q.data[q.front];
        q.front  = (q.front+1)%MaxSize;
        return true;
    }
```





