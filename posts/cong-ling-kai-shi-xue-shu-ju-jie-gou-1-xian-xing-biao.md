---
title: '从零开始学数据结构之线性表'
date: 2019-11-10 20:50:01
tags: [数据结构]
published: true
hideInList: false
feature: https://tva4.sinaimg.cn/large/0072Vf1pgy1fodqo0e8fzj31hc0xcqv5.jpg
---
线性表是一种线性存储结构的，除了首位元素，每一个元素都有一个直接前驱；除了末尾元素，每一个元素都有一个直接后继。

<!-- more -->

```
 // 对于线性表的常用操作如下
	 InitList(&L)  初始化线性表
	 Lenght(&L) 返回表长
	 LocateElem(&L,e) 按值来查找元素，在表L中查找指定元素
	 GetElem(&L,i) 按序号查找元素，在表L查找指定位置的元素
	 ListInsert(&L,i,e) 将元素e插如表L的指定位置i处
	 ListDelete(&L,i,&e) 将表L的第i个元素删除并用e返回删除元素的值
	 PrintfList(&L) 打印线性表L
	 Empty(&L) 判断线性表L是否为空，空则返回true，否则返回false
	 DestoryList(&L) 销毁线性表L
```

# 1.顺序表
>顺序表是一种顺序存储结构的线性表，顺序表的逻辑相邻的两个元素在存储在物理结构上也相邻。
### 1. 顺序表的数据结构
```c++
		#define MaxSize 50
		typedef struct sq *SqList;
		struct sq {
				ElemType data[MaxSize];
				int Length;
		};
		typedef SqList SL;
```

### 2.    顺序表的常用操作
#### 	按值查找
```c++

	int LocateElem(SL &L, ElemType e){
			if(!L->Length){
					return 0;
			}
			for(int i = 0; i< L->Length; i++){
					if(L->data[i] == e){
							return i+1;  // 线性表的元素是从下标为1开始计算，而数组是从下标0
							//开始计算，此处返回元素下标时应该加上1才能对应元素在线性表中的
							//下标
					}
			}
			return 0;
	}
```
#### 按序号查找

```c++
	
	ElemType GetElem(SL &L,int i, ElemType e){
			if(i == 0){
					e = L->data[0];
					return e;
			}
			if(!L->Length || i<1 || i > L->Length){
					return e;
			}
			e = L->data[i-1];
			return e;
	}
```

#### 插入操作

```
		int ListInsert(SL &L, int i, ElemType e)
		{
				if (L->Length == MaxSize)  // 线性表已满 不允许插入操作
				{
						return 0;
				}
				if (i < 1 || i > L->Length+1)  // 超出线性表当前范围 不允许插入操作
				{
						return 0;
				}
				for (int j = L->Length; j >= i; j--)
				{
						L->data[j] = L->data[j - 1];
				}
				L->data[i - 1] = e;
				L->Length++;
				return 1;
		}
```

#### 删除操作

```
int ListDelete(SL &L, int i, ElemType &e)
{
    if (L->Length == 0 || i > L->Length + 1)
    {
        return 0;
    }
    e = L->data[i - 1];
    for (int j = i - 1; j < L->Length; j++)
    {
        L->data[j] = L->data[j + 1];
    }
    L->Length--;
    return 1;
}
```

#  2.链表
>由于顺序表的插入和删除操作需要移动大量的元素，影响运行效率，线性表的链式结构存储时不需要逻辑上相邻的两个元素在物理位置上也相邻。
### 		1.单链表的数据结构
```c++
	typedef struct LNode *LinkList;
	struct LNode {
			ElemType data;
			LNode *next;
	};
	typedef LinkList List;
```
2. 单链表的常用操作
#### 创建单链表
1. 头插法创建

```
// 头插法
List HeadInsert(List &L)
{
    List p, s; // p为头指针
    int n = 20, i = 1;
    p = (List)malloc(sizeof(struct LNode));
    p = L;
    p->next = NULL; // 初始为空表
    // scanf("%d",&n);
    while (i < n)
    {
        s = (List)malloc(sizeof(struct LNode));
        s->data = i;
        s->next = p->next;
        p->next = s;
        i++;
    }
    return p;
}
```
2. 尾插法

```
//  尾插法
List TailInsert(List &L)
{
    List p, s; // 此处p为尾指针
    int n = 10, i = 1;
    p = (List)malloc(sizeof(struct LNode));
    p = L;
    // scanf("%d",&n);
    while (i < n)
    {
        s = (List)malloc(sizeof(struct LNode));
        s->data = i;
        p->next = s;
        p = s;
        i++;
    }
    p->next = NULL; // 尾节点指针置空
    return L;
}
```


####  	按值查找
```c

	List LocateElem(List &L, ElemType e){
			List p = (List)malloc(sizeof(struct LNode));
			p = L;
			while(p && p->data != e){
					p = p->next;
			}
			return p;
	}

```

#### 按序号查找
> 返回目标节点指针
```
List GetElem(List &L, int i)
{
    LNode *p = (List)malloc(sizeof(struct LNode));
    p = L->next; // 头节点指针赋值给p
    int j = 1;
    if (i == 0)
    {
        return p;
    }
    if (i < 1)
    {
        return NULL; // 若i 小于1且不为0，则i无效，返回NULL；
    }
    while (p && j < i)
    {
        j++;
        p = p->next;
    }
    return p;
}
```

#### 插入元素
> 插入元素只需要修改指针指向即可
```
List ListInsert(List &L, int i, ElemType e)
{
    List p, s;
    s = (List)malloc(sizeof(struct LNode));
    s->data = e;
    if (i == 0 || i == 1)
    {
        p = L;
    }
    else if (i < 1)
    {
        return NULL;
    }
    else
    {
        p = GetElem(L, i - 1);
    }
    s->next = p->next;
    p->next = s;
    return s;
}
```
> tip:如果已知当前元素的指针，对于前插操作，链表需要遍历整个链表，此时的时间复杂度为O(n),如果换个思路，可以将元素插入目标位置之后，然后将前后两元素内部的数据进行互换同样可以完成目标✅，而此时的时间复杂度为O(1)
	
####  删除元素

```
List ListDelete(List &L, int i)
{
    List p, s;
    s = (List)malloc(sizeof(struct LNode));
    if (i == 0 || i == 1)
    {
        p = L;
    }
    else if (i < 1)
    {
        return NULL;
    }
    else
    {
        p = GetElem(L, i - 1);
    }
    s = p->next;
    p->next = s->next;
    free(s);
}
```

线性表除了可以用顺序表、单链表之外还有更加方便各种操作的**双链表**、**循环链表**、**循环单链表**、**循环双链表**、**静态链表**等等。总之线性表是基础、超级重要啊啊啊！！！




