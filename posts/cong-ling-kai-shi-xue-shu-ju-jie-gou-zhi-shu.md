---
title: '从零开始学数据结构之树（一）'
date: 2019-11-21 21:26:21
tags: [数据结构]
published: true
hideInList: false
feature: https://tva4.sinaimg.cn/large/0072Vf1pgy1foxk3yu6vwj31kw0w04k5.jpg
---
> 树，是一种递归的数据机构，具有一种分层的逻辑结构，适用于表示具有层次关系结构的数据；


<!-- more -->

### 1.树的定义
树是具有n的节点的有限集合，当n=0时，表示空树。任何树都应该具有：
1. 当树非空时，一棵树有且仅有一个根节点
2. 除根结点外任一元素只有一个前驱节点
3. 所有节点都可以具有零个或多个后继节点

### 2.二叉树
定义：每个节点仅有两个后继节点，并且后继节点有左右之分，是有次序之分。

#### 1.二叉树的链式存储结构

```c++
    typedef int ElemType;
    typedef struct BitNode *BitTree;
    struct BitNode
    {
        ElemType data;
        BTree lchild, rchild;
        /* data */
    };
    typedef BitTree BTree;

```
ps:在二叉树的链式表示中，若存在n个节点，则具有n+1个空链域。

#### 2.二叉树的遍历
二叉树的遍历分为先序遍历、中序遍历、后序遍历以及层次遍历。
    * 先序遍历：先访问树的根结点再先序遍历该节点的的左子树最后先序遍历该节点的右子树；即顺序为根->左->右
    * 中序遍历：先中序遍历根节点的的左子树再访问树的根结点最后中序序遍历该节点的右子树；即顺序为左->根->右
    * 后序遍历：先后序遍历根节点的的左子树再后序序遍历该节点的右子树最后访问树的根结点；即顺序为左->右->根
```c++
    void PreOrder(BTree &T)
        {
            if (T != NULL)
            {
                // Visit(T);  先序遍历
                PreOrder(T->lchild);
                // Visit(T);  中序遍历
                PreOrder(T->rchild);
                // Visit(T);  后序遍历
            }
        }
```
以上为使用递归的方式完成的先、中、后序的遍历，除了使用递归的方式外还可以使用循环的方式来实现遍历，此时就需要借助栈来实现


```c++
void InOrder(BTree T)
{
    Stack s;
    InitStack(s); // 初始化栈
    BTree t = T;
    while (t || !StackEmpty(s)) // 节点存在或者栈不为空执行循环
    {
        if (t)  // 若节点存在，将节点压入栈中，之后遍历左子树
        {
            Push(s, t);  
            t = t->lchild;
        }
        else  // 若节点不存在，将节点从栈中弹出，访问，之后遍历右子树
        {
            Pop(s, t);
            Visit(t);
            t = t->rchild;
        }
        /* code */
    }
}
```

以上为中序遍历的非递归实现方式，其中是借助栈来实现的，而对于层次遍历，则需要借助队列来实现
```c++

    void LevelOrder(BTree &T)
    {
        SqQueue q;
        InitQueue(q);
        BTree t = T;
        EnQueue(q, t);  // 先将根节点入队，
        while (!QueueEmpty(q))
        {
            DeQueue(q, t); // 将结点出队，于此同时将该节点的左右子树的根结点入队 直到队列为空
            Visit(t);
            if (t->lchild)
            {
                EnQueue(q, t->lchild);
            }
            if (t->rchild)
            {
                EnQueue(q, t->rchild);
            }
        }
    }
```

以上对于二叉树的层次遍历使用到了队列来实现。

#### 3.线索二叉树
在上面说过二叉树的链式存储时n个节点会产生n+1个空链域。线索二叉树就是将这些闲置的空链域利用起来，约定：若节点左子树为空，那么将其左子树指针指向其前驱元素，如柚子树为空，将其指针指向其后继节点，这个过程叫做二叉树的线索化。
```c++
    typedef int ElemType;
    typedef struct BitNode *BitTree;
    struct BitNode
    {
        ElemType data;
        BTree lchild, rchild;
        int ltag,rtag; //添加字段ltag，rtag。 0 表示有左右孩子，1表示指向前驱或后继
        /* data */
    };
    typedef BitTree BTree;

    // 中序遍历线索化
    void InOrder(BTree &T,BTree &pre){ // pre表示其前驱节点
        if(T!= NULL){
            if(T->lchild == NULL){ // 左子树为空，则指向前驱节点
                T->ltag = 1;
                T->lchild = pre;
            }
            if(pre != NULL && pre->rchild == NULL){ // 前驱节点右子树为空，则指向当前节点
                pre->rtag = 1;
                pre->rchild = T;
            }
            pre = T;
            InOrder(T->lchild,pre);
            InOrder(T->rchild,pre);
        }
    }

    // 创建线索二叉树
    void CreateInThread(BTree &T){
    BTree pre = NULL;  // 初始值设置为NULL
    if(T){
        InThread(T,pre);
        pre->rchild = NULL; // 初始值右节点指向空
        pre->rtag = 1;
    }
}

```




