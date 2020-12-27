---
title: 'Javascript 中的 new 操作符'
date: 2020-03-30 10:50:18
tags: []
published: true
hideInList: true
feature: https://tva2.sinaimg.cn/large/0072Vf1pgy1foxk6mvbgfj31kw0w01cd.jpg
isTop: false
---
在 js 中 new 操作符到底实现什么，在经过网上查询之后我总结了一下，留做笔记。

首先，试一下原生 js 中的 new 的作用

```javascript
function Person(name , age) {
    this.name = name,
    this.age = age
}

Person.prototype.sayName = function(){
    console.log(this.name)
}
let per = new Person( '小明', 12 )

console.log(per)
per.sayName()

// Person { name: '小明', age: 12 }
// 小明

```

由上面可以看出 js 中的 new 操作符在 new 的过程中：
1. 创建一个以构造函数的原型为原型的对象即 Object.create(Person.prototype)
2. 将构造函数的 this 绑定到上一步所创建的对象上
3. 返回创建的对象
   用代码实现：
```javascript

function myNew(parent , ...arg){
    // 创建一个以构造函数的原型为原型的空对象
    let obj = Object.create(parent.prototype);
    // 绑定构造函数的 this 到新对象上
    parent.apply(obj,arg)
    // 返回新创将的对象
    return obj
}

let per1 = myNew(Person,'小明', 12)

console.log(per)
per.sayName()
// Person { name: '小明', age: 12 }
// 小明


```