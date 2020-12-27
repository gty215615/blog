---
title: '从零开始学数据结构之栈'
date: 2019-11-13 20:51:22
tags: [数据结构]
published: true
hideInList: false
feature: https://tva4.sinaimg.cn/large/0072Vf1pgy1foxlhbqhasj31hc0u0qjw.jpg
---
栈是一种只允许在一端进行插入和删除的线性表，栈也是一种线性表,栈的特点是后进先出(Last In First Out)。

<!-- more -->


## 顺序栈

### 首先来确定栈的数据结构

```c++
    #define MaxSize 50
    typedef int ElemType;
    struct SqStack{
        ElemType data[MaxSize];  
        int top; // 栈顶指针，初始化时会设置为-1.
    } ;
```

然后来确定栈的一些特殊临届状态值：
1. 栈空状态：约定当栈顶指针s.top == -1 时，表示此时为空栈，不允许再进行出栈操作。
2. 栈满状态：当栈顶指针s.top == MaxSize-1 时，表示此时栈已满，不允许再进行入栈操作。

然后就可以来进行实现常用的对于栈的操作

#### 初始化空栈
```c++
    void InitStack(SqStack &s){
        s.top = -1;
    }
```
#### 判断是否为空栈
```c++
    bool StackEmpty(SqStack &s){
        if(s.top == -1){
            return true;
        }else{
            return false;
        }
    }
```

#### 入栈操作
> 入栈时将栈顶指针s.top+1,再将新元素入栈。

```c++
    bool Push(SqStack &s,ElemType x){
        if(s.top == MaxSize-1){
            return false;
        }
        s.data[++s.top]=x;
        return true;
    }
```
#### 出栈操作
> 出栈时将元素出栈后再将栈顶指针减去一。
```c++
    bool Pop(SqStack &s,ElemType &x){
        if(s.top == -1){
            return false;
        }
        x = s.data[s.top--];
        return true;
    }
```

#### 获取栈顶元素
```c++
    bool GetTop(SqStack &s,ElemType &x){
        if(s.top == -1){
            return false;
        }
        x = s.data[s.top]
        return true;
    }
```

除此之外还有共享栈，利用栈底不变的特性，将两个栈放在同一个一维数据空间内部以达到共享空间的目的。还有栈的链式存储结构与线性表的链式存储结构类似，只是具体实现方面有所不同。



















