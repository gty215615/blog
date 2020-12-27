---
title: 'Javascript基本数据类型之Symbol'
date: 2020-01-06 10:37:12
tags: [Javascript]
published: true
hideInList: false
feature: https://tva4.sinaimg.cn/large/0072Vf1pgy1foxkj5gmcmj31hc0u0wvj.jpg
---
> Symbol类型是ES6新添加的一种基本数据类型，具有自己的静态属性与静态方法。并且其静态方法会暴露出全局的Symbol注册表。Symbol是一个不完整的构造函数，因为他不能通过new Symbol()来创建Symbol的实例。Symbol创建的值是唯一的，所以它仅有的目的就是为对象作为私有属性的标识符。

1. Symbol创建实例,且每个Symbol的实例都是独一无二的。
   
```javascript
var symbol = Symbol();
var symbol1 = Symbol("foo");
console.log(Symbol("aaa") === Symbol("aaa"))
// Symbol()
// Symbol(foo)
// false

```

2. Symbol.for(token) 与 Symbol.keyFor(Symbol);
   Symbol.for(token) 方法回去Symbol全局注册表中查找是否存在有传入的token字符串所生成的Symbol实例，若有则会返回找到的Symbol实例；若没有则会创建一个新的Symbol实例注册到全局的Symbol注册表中并返回新生成的Symbol实例。
```javascript
var a = Symbol.for("a");
var e = Symbol.for("a");
console.log(a === e)
// true
```
   Symbol.keyFor(Symbol)方法，在全局注册表中寻找传入的Symbol实例是否存在，若存在则返回生成该实例的token字符串，若不存在，则返回undefined。
```javascript
var a = Symbol.for("a");
console.log(Symbol.keyFor(a) === "a")
// true
```
3. Symbol作为对象的属性的标识符时，是不可以被枚举的，对象的getOwnPropertyNames方法也不能取到该属性，可以通过getOwnPropertySymbols方法来取到对象的全部以Symbol实例作为标识符的属性。
```javascript
var age = Symbol.for("age");
var obj = {};
obj[age] = 20;
obj["name"] = "小明";
obj["sex"] = "男";
for(var k in obj){
    console.log(obj[k]);
}
console.log(Object.getOwnPropertyNames(obj));
console.log(Object.getOwnPropertySymbols(obj));
// 小明
// 男
// [ 'name', 'sex' ]
// [ Symbol(age) ]
   ```




































