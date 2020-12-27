---
title: 'call、apply与bind的区别'
date: 2020-01-05 08:32:09
tags: [Javascript]
published: true
hideInList: false
feature: https://tva4.sinaimg.cn/large/0072Vf1pgy1foxkfkejbbj31hc0u0k7e.jpg
---
> bind 强制改变函数内部this指向，并返回一个函数，故其并不会立即执行
> apply 强制改变函数内部运行时this指向，立即执行，其第二个参数为一个数组
> call 强制改变函数内部运行时this指向，立即执行，其后从第二个参数开始，每一个参数将传入函数中作为参数执行。
1. call 函数实现
    
首先，call函数有两个基础功能，一，会将函数的this指向为目标对象，如果没有目标对象就会指向window（浏览器中）全局对象，第二个就是call会立即执行。因此可以将其看做事将这个目标对象添加一个属性指向当前函数然后立即执行，最后将其添加的属性删除。

```javascript   
    var obj = {
    name:1
    }
    function getname(sex,age) {
    console.log(this.name)
    }
    getname.call(obj) //  1
    // this指向改变了
    // getname函数被执行了
```

接着，call函数还可以向函数中添加参数，可以通过函数的默认arguments属性来获取函数接收到的属性，代码如下

```javascript
    function getname(sex,age) {
    console.log(this.name)
    console.log(sex)
    console.log(age)
    }
    Function.prototype.call3 = function (context) {
    context.fn = this;
    var args = []
    for(var i = 1;i<arguments.length;i++){
    args.push("arguments["+i+"]")
    }
    eval("context.fn("+args+")") 
    delete context.fn
    }
    getname.call3(obj,"男",25)  // 1 男  25
```
2. 对于apply实现方式与call类似，只不过要判断第二个参数是否存在，实现代码如下
```javascript
    Function.prototype.myapply = function (context) {
    context.fn = this;
    var arr = arguments[1]
    var args = []
    if(!arr){
        context.fn()
    }else{
        for(var i = 0;i<arr.length;i++){
            args.push("arr["+i+"]")
        }
        eval("context.fn("+args+")") 
    }
    delete context.fn
}

```
3. bind的实现，bind的实现是在apply的基础上实现的，先来分析bind有哪些功能
    1. 首先bind的会返回一个函数，它不会立即执行
    2. 其次bind可以传入参数
    3. bind的返回值可以当做一个构造函数，调用new方法可以生成实例
    第一步先实现会返回一个函数，执行可以看见改变this后指向的结果
```javascript
    obj = {
    name:"Mosh"
    }
    function getInfo(params) {
    console.log(this.name)
    }
    Function.prototype.myBind = function (context) {
    const _self = this;
    return function fn(params) {
    _self.apply(context)
    }
    }
    var objFn = getInfo.myBind(obj)
    objFn()  // Mosh

```
由于原生js的bind实现方法可以在调用bind的方法的时候传入参数，也可以在调用bind返回函数执行的时候传入参数，因此要讲两次的参数组合起来，代码如下

```javascript
    obj = {
    name:"Mosh"
    }
    function getInfo(sex,age) {
    console.log(this.name)
    console.log(sex)
    console.log(age)

    }

    Function.prototype.myBind = function (context) {
    const _self = this;
    const args = [].slice.call(arguments,1);
    return function fn(params) {
    _self.apply(context,args.concat([].slice.call(arguments)))
    }
    }
    var objFn = getInfo.myBind(obj,15)
    objFn("男") 
    //  Mosh
    //  15
    //  男
```
然后还有一个最重要的就是构造函数模式的实现，在该模式下由bind模式生成的函数通过new 方法构造的实例，会继承原函数上的所有属性，以及传入的全部参数，只是绑定的this指向失效，重新指向了新生成的实例上，实现代码如下：
```javascript
    obj = {
    name:"Mosh"
    }
    function getInfo(sex,age) {
    console.log(this.name)
    console.log(sex)
    console.log(age)
    }
    getInfo.prototype.type = 'function'
    Function.prototype.myBind = function (context) {
    const _self = this;
    const args = [].slice.call(arguments,1);
    function Fn(params) {
    _self.apply( this instanceof Fn ? this : context,args.concat([].slice.call(arguments)))
    }
    var NewFn = this.prototype
    Fn.prototype = new newFn()
    return Fn
    }
    var objFn = getInfo.myBind(obj,15)
    var newObj = new objFn("男")
    console.log(newObj.type);
    //undefined
    //15
    //男
    //function

```
最后，就是一些细节处理。如果调用bind的不是函数，要报错，完整代码如下：
```javascript
    Function.prototype.myBind = function (context) {
    if(typeof this !== "function"){
    throw new Error();
    }
    const _self = this;
    const args = [].slice.call(arguments,1);
    function Fn(params) {
    _self.apply( this instanceof Fn ? this : context,args.concat([].slice.call(arguments)))
    }
    var NewFn = this.prototype
    Fn.prototype = new newFn()
    return Fn
    }
```

    