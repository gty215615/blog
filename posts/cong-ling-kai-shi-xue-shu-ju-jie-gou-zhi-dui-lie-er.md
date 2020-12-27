---
title: '从零开始学数据结构之队列（二）'
date: 2019-11-18 18:04:57
tags: [数据结构]
published: true
hideInList: false
feature: https://tva1.sinaimg.cn/large/0060lm7Tly1fpx1skrabaj31hc0u01kx.jpg
---
之前学习了队列的顺序存储结构的代码实现，以及循环队列的实现，队列还可以使用链式存储结构来实现，并且链式存储的队列不会出现“溢出”的现象。

<!-- more -->

1. 数据结构如下

```c++
        typedef int ElemType;

            typedef struct LinkNode *LNode;
            struct LinkNode
            {
                ElemType data;
                struct LinkNode *next;
            };

            typedef struct LinkQueue *LQueue;
            struct LinkQueue
            {
                LinkNode *front, *rear;
            };         

```
2. 具体代码实现

    1. 队列初始化

```c++
        void InitQueue(LQueue &q)
            {
                LNode L = (LNode)malloc(sizeof(LNode));
                L->next = NULL;
                q->front = q->rear = L;
            }
```

2. 判断队列是否为空

```c++
        bool QueueEmpty(LQueue &q)
            {
                if (q->front == q->rear)
                {
                    return true;
                }
                return false;
            }
```

3. 入队

```c++
        bool EnQueue(LQueue &q, ElemType e)
                {
                    LNode L = (LNode)malloc(sizeof(LNode));
                    L->data = e;
                    L->next = q->rear->next;
                    q->rear->next = L;
                    q->rear = L;
                    return true;
                }
```

4. 出队
```c++
        bool DeQueue(LQueue &q, ElemType &e)
                {
                    if (q->front == q->rear)
                    {
                        return false;
                    }
                    LNode L = (LNode)malloc(sizeof(LNode));
                    L = q->front->next;
                    e = L->data;
                    q->front->next = L->next;
                    if (L == q->rear)
                    {
                        q->front = q->rear;
                    }
                    free(L);
                    return true;
                }
```



5. 读取队头元素

```c++
            bool GetHead(LQueue &q, ElemType &e)
                {
                    bool flag = QueueEmpty(q);
                    if (flag)
                    {
                        return false;
                    }
                    e = q->front->data;
                    return true;
                }
```

队列的链式存储适用于数组元素变动比较大的情形下，而且不存在队列满而产生的数据溢出的情况。
    


