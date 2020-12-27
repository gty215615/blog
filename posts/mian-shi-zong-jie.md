---
title: '面试总结'
date: 2020-02-24 16:19:44
tags: []
published: true
hideInList: true
feature: https://tva4.sinaimg.cn/large/87c01ec7gy1frmmnrncphj21hc0u07wj.jpg
isTop: false
---

## 盒模型

1. 标准盒模型
    盒子的实际宽度 = width + padding + border + margin
    width = content
2. IE 的怪异盒模型
    盒子的实际宽度 = width + margin 
    width = content + padding + border
3. 可以使用 box-sizing属性来切换盒模型

## 布局

1. 定位布局 （position： static 、 relative 、 absolute 、 fixed 、 sticky  ）
    > 1. 默认为 static
    > 2. relative ： 相对定位 相对于自身之前的位置移动来进行定位
    > 3. absolute ： 绝对定位 相对于父级部位 static 的元素来进行定位
    > 4. fixed：固定定位 相对于浏览器窗口来进行定位
    > 5. sticky：粘性定位 固定定位于绝对定位的结合  
2. 流布局，就是使用百分比布局，通常配合 max-/min-来进行约束， 根据浏览器窗口的分辨率来进行调节显示元素的大小，
使得整体布局不变 。
    缺点： 在屏幕过大或过小情况下导致元素显示不正常 
3. 浮动布局 ：使用 float 属性，使元素脱离文档流浮动，但不会脱离文本流，因此可以
实现文字环绕的效果，会导致很多问题
    常用清除浮动方法：
    > 1. 额外标签法：在父元素末尾添加一个空标签加上 clear：both；
    > 2. 触发BFC overflow：hidden；会导致溢出部分被隐藏
    > 3. 使用 css3 的伪元素来清除浮动
4. 圣杯布局 ： 利用负边距于相对定位来进行布局，形成左右两边固定中间位置自适应的三栏布局
5. flex 布局
```css
     1. 父元素设置flex 属性后，子元素的float、clear、vertical-align 将失效
     2. flex-direction:  决定主轴方向
        1. row 从左往右 ，
        2. row-reverse 从右往左
        3. column 从上往下
        4. column-reverse 从下往上
     3. flex-wrap: 元素换行方式
        1. nowrap （默认）不换行
        2. wrap 换行，第一行在上方
        3. wrap-reverse 换行，第一行在下方
     4. justify-content 在主轴上的对齐方式
        1. flex-start（默认值）：左对齐
        2. flex-end：右对齐
        3. center： 居中
        4. space-between：两端对齐，元素之间的间隔都相等。
        5. space-around：每个元素两侧的间隔相等。所以，元素之间的间隔比元素与边框的间隔大一倍。
     5. align-items 交叉轴上的对齐方式
        1. flex-start：交叉轴的起点对齐。
        2. flex-end：交叉轴的终点对齐。
        3. center：交叉轴的中点对齐。
        4. baseline: 元素的第一行文字的基线对齐。
        5. stretch（默认值）：如果元素未设置高度或设为auto，将占满整个容器的高度。 
     6. align-content 多根轴线的对齐方式，若只有一根轴线则不起作用
     7. flex-grow 元素的放大比例 
     8. flex : 是flex-grow（放大比例）, flex-shrink（缩小比例） 和 flex-basis（基础占用空间）
     9. align-self: 可以用来覆盖 align-items 的属性，来设置单个元素的对齐方式
```
6. grid 布局
```css
     1. 父元素设置grid 属性后，子元素的float、display: inline-block、display: table-cell、vertical-align和column-* 将失效
     2. grid-template-columns:指定每列列宽，grid-template-rows：指定每行行高
     3. auto-fill 自动填充，直到装不下下一个单元格
     4. fr( fraction:片段 ) 是单元格比例关系
     5. grid-gap 设置行列间隔，为 grid-row-gap 与grid-column-gap 的缩写
    待续。。。
```
7. BFC 块格式化上下文 (Block Formatting Context) 
    > 1. 创建：
            1. 浮动元素 (元素的 float 不是 none)
            2. 绝对定位元素 (元素具有 position 为 absolute 或 fixed)
            3. 内联块 (元素具有 display: inline-block)
            4. 表格单元格 (元素具有 display: table-cell，HTML表格单元格默认属性)
            5. 具有overflow 且值不是 visible 的块元素，
    > 2. 作用：
            1. 清除浮动
            2. 产生边界(与浮动元素)
            
8. 数据存储 （ cookie、sessionStorage、localStorage ）
     >1. cookie : 会话 cookie 不设置过期时间，关闭浏览器窗口失效
                  设置过期时间，超过时间，cookie 失效
                 特点： 每次请求浏览器会将 cookie 通过 http 的请求头发送给服务端，安全性能低，浪费带宽，且 cookie只能存储 4kb 数据，
                 还只能是文本字符串
                 
     >2. sessionStorage : 浏览器本地存储，大小 5M ，当浏览器窗口关闭则失效，也是会话级存储，
     >3. localStorage: 浏览器本地存储，大小 5M，浏览器窗口关闭不会消失，除非手动清除，否则永久存在。
     >4. 相对于 cookie 而言 webStorage的安全性更高且不会发送给服务器，并且浏览器有提供操作的 api，更加便于操作。
9.  animation和transiton的相关属性 
     >1. animation ( 动画名称，动画播放时间，动画速度曲线，动画延迟时间，动画播放次数，动画是否反向播放 )
     >2. transiton ( 过度属性名称，过度时间，过度曲线 ，过度延迟时间 )

# JavaScript
    
## 数据类型
1. 基础数据类型
```javascript
        1. number
            - typeof NaN  // number
        2. string
        3. boolean
        4. symbol
            - Symbol 实例是唯一存在的的
            - 主要用来作为对象键名的标识符
            - 以 Symbol 作为标识符的键名，不可以被枚举，可以通过 getOwnPropertySymbols 来查询到
        5. undefined
            - typeof undefined  // undefined
        6. null
            - typeof null // object
```
2. 引用数据类型
```javascript
        1. Object
        2. Array
        3. Function
        ...
```
##  作用域
1. 作用域链：
        1. 在 JavaScript 中函数会形成一个单独的作用域，整个作用域是由全局作用域以及每一个函数形成的作用域所组成，在外部的作用域不能访问内部作用域
        中的变量，而在内部作用域可以访问其外部作用域中的变量，在 js 中，若调用一个变量，会首先在当前作用域中查找，如果没找到就会去上一级作用域中查找
        一直到查找到顶级也就是全局作用域，若没找到，则会提示' 变量名 is not defined' ，在这每一级的查找过程中，作用域形成了链式结构，便叫做作用域
        链
    2. 闭包：闭包是由函数以及创建该函数的词法环境所组成的，这个词法环境就是这个函数创建时所能访问到的所有的局部变量。
        1. 闭包作用：
            1. 常用于变量的私有化。
            2. 保存外部变量，被闭包所引用的外部变量不会被 js 引擎回收
        2. 缺点：
            1. 大量使用闭包会造成内存泄漏，影响性能，造成网页卡顿。 ( 解决方法： 手动删除被闭包引用的局部变量 )
##  JavaScript继承

1. 构造函数继承
    >  使用 call、apply 来实现
```javascript
            function Person(name){
                 this.name = name;
                 this.species = 'people'
            }
            function Man(name ,sex){
                Person.apply(this,arguments);
                this.sex = sex
            }
            const a = new Man('a','男')
            
            console.log(a)  // Man { name: 'a', species: 'people', sex: '男' }
```  
缺点： 
    1. 每个实例都拷贝一份父类的方法，占用内存大，尤其是方法过多的时候，因此可以用原型继承来解决占用内存过大的窘境）
    2. 当需求变更要修改方法时，之前所生成的实例方法是无法及时更新的。
            
        
        
2. 原型继承   
    1. 原型链： 
```javascript
                function Person(name){
                       this.name = name;
                }
                Person.prototype.color = 'yellow';
                
                // 构造函数 Person 的prototype 属性指向 一个对象，这个对象就是 Person 的原型，
                const mash = new Person('mash');
                console.log(mash.name) // mash
                console.log(mash.color) // yellow
                
                //  由构造函数 Person 生成的实例 mash 拥有 color 属性， 这个属性继承自 Person.property
                //  即实例会继承原型上的属性。 与此同时，实例 mash 的__proto__属性指向构造函数原型，可以访问到原型上的属性
                //  由于构造函数的原型 Person.prototype 是一个对象，它也会拥有创造它的构造函数，也会有原型，因此它也可以访问它的原型上的属性，
                //  由此一层一层往上形成一条原型链，原型链的顶端为 null（null 在js 中也是一个对象）。
                
                //  在js 中查找对象的属性时有一条这样的查找规则:当对象本身没有这个属性的时候，js 引擎会查找该对象的原型，并顺着原型链一级一级的找上去
                //  直到找到 null 为止，如果没找到，则会显示为 undefined；
```
  1. 原型继承：即将子类的原型链接到父类的实例上即可实现原型继承 
```javascript
                  function Person(name){
                          this.name = name;
                  }
                  Person.prototype.sayHello = function(){ console.log('Hello '+ this.name) } ;
                  
                  function Student(level){
                        this.level = level;
                  }
                  
                  Student.prototype = new Person( '小明' )
                  Student.prototype.constructor = Student;
                  const x = new Student(1)
                  
                  console.log(x.name) // 小明
```
缺点： 在上方的实现中重写了 Student 的原型指向，这就会导致重写之前所创建的实例仍然指向之前的原型.
             
3. 组合继承
    > 为了解决以上两种继承所展现出的问题，可以使用组合的方式来避免他们各自的缺陷。
```javascript
             function Person(name){
                  this.name = name;
             }
             Person.prototype.sayHello = function(){ console.log('Hello '+ this.name) } ;
                              
             function Student(level){
                    Person.apply(this,arguments)  // 第二次调用构造函数
                    this.level = level;
             }
                              
             Student.prototype = new Person( '小明' )    // 第一次调用构造函数
             Student.prototype.constructor = Student;
             const x = new Student(1) 
                              
             console.log(x.name) // 小明
```
缺点：无论在什么情况下，都会调用两次父类构造函数：一次是在创建子类型原型时，另一次是在子类型构造函数内部。  
           
4. 寄生继承 : 寄生继承就是不用实例化父类了，直接实例化一个临时副本实现了相同的原型链继承。（即子类的原型指向父类副本的实例从而实现原型共享）
```javascript
            function Training(child,person){
                const newPerson = function(){};
                newPerson.prototype = person.prototype
                child.prototype = new newPerson()
                child.prototype.constructor = child;
            }
            function Person(name){
                this.name = name;
            }
            Person.prototype.sing = function () {
                console.log('sing song')
            }
             function Student(age){
                  this.age = age;
             }
            Training(Person)
            const b = new Student(1)
            
            b.sing()
```
5. 寄生组合继承： 
    1. 使用构造函数方式继承属性
    2. 使用原型继承方式继承方法
    3. 使用寄生继承方式避免调用两次父类构造函数
```javascript
              function extend(child,parent){
                             const newPerson = function(){};
                             newPerson.prototype = person.prototype
                             child.prototype = new newPerson()
                             child.prototype.constructor = child;
                         }
                         function Person(name){
                             this.name = name;
                         }
                         Person.prototype.sing = function () {
                             console.log('sing song')
                         }
                        function Student(name,level){
                            Person.call(this,arguments);
                            this.level=level
                        }
                        extend(Student,Person)
```
                              
## 前端跨域解决：（JSONP，CORS）  
HTTP 响应中包含一个状态码，用来表示服务器对客户端响应的结果。
状态码一般由3位构成：

1xx : 表示请求已经接受了，继续处理。
2xx : 表示请求已经处理掉了。
3xx : 重定向。
4xx : 一般表示客户端有错误，请求无法实现。
5xx : 一般为服务器端的错误。

比如常见的状态码：

200 OK 客户端请求成功。
301 Moved Permanently 请求永久重定向。
302 Moved Temporarily 请求临时重定向。
304 Not Modified 文件未修改，可以直接使用缓存的文件。
400 Bad Request 由于客户端请求有语法错误，不能被服务器所理解。
401 Unauthorized 请求未经授权，无法访问。
403 Forbidden 服务器收到请求，但是拒绝提供服务。服务器通常会在响应正文中给出不提供服务的原因。
404 Not Found 请求的资源不存在，比如输入了错误的URL。
500 Internal Server Error 服务器发生不可预期的错误，导致无法完成客户端的请求。
503 Service Unavailable 服务器当前不能够处理客户端的请求，在一段时间之后，服务器可能会恢复正常。

作者：只会番茄炒蛋
链接：https://juejin.im/post/5e3d898cf265da5732551a56
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。   
1. 跨域的原因：
由于浏览器的同源策略即当（端口、协议、域名）相同时两个脚本才可以进行交互
2. JSONP 实现跨域
JSONP原理： 
> 利用script标签不受同源策略的限制，向后端请求 js 文件，后端返回的 js 文件内包含一个函数的调用，并且会传回前端所需要的数据作为参数，只需现在前端页面预先写好需要后端执行的函数体即可
3. CORS（跨域资源共享）
    > 简单请求：
    > 请求方法（ HEAD，GET，POST ）
    > http 头信息字段  （ Accept，Accept-Language，Content-Language， Last-Event-ID，Content-Type （不为 application/json）  ） 
    > 同时符合以上两种情况都为简单请求
                
1. CORS 是在 http 请求头上添加额外的信息，使得 web 可以被允许访问不同源的资源+
2. 头部信息：
    - request：
    1. 对于简单请求：
        浏览器会在 http 请求头上加一个 origin 的字段传递当前的源信息（域名、端口、协议），若源被后端服务器允许则响应头 会被加上
        Access-Control-Allow-Origin 的字段，若不被服务器允许，则无此字段，会被XMLHttpRequest的onerror回调函数捕获
    - response：
    1. Access-Control-Allow-Origin： 用来指定那些域名的客户端可以访问本服务器上的资源，当请求头携带有 cookie 时不可以使用*，
    2. Access-Control-Allow-Credentials ： 若服务器支持通过 cookie传递验证信息，则该选项变回存在的返回数据里，只有一个值就是true，于此同时在前端也要打开withCredentials字段,即：
```javascript
   var xhr = new XMLHttpRequest();
   xhr.withCredentials = true;
```
否则即使服务器同意发送 cookie，浏览器也不会发送
1. Access-Control-Allow-Headers 服务器支持的请求数据类型
2. Access-Control-Allow-Methods 服务器支持的请求类型
对于非简单请求，浏览器会发送一个预检请求，当预检请求通过后服务端会返回一个Access-Control-Max-Age字段，表示预检请求的过期时间，在此时间内，非简单请求与简单请求一样
                    
相比于 JSONP，CORS功能更强大，支持方法更多，JSONP 仅仅支持 GET 方法，JSONP 优势在与某些不支持 CORS 的老版本浏览器
            
## 节流、防抖   
1. 节流 （throttle）： 事件在一定时间内仅仅执行一次
                
```javascript
            function throttle(fn ,wait){
                let timer = true
                return function () {
                    if(!timer){
                        return
                    }
                    timer = false;
                    setTimeout(()=>{
                        fn.apply(this,arguments)
                        timer = true;
                    },wait)
                }
            }
```
2. 防抖（debounce）： 触发事件后一定时间内仅执行一次，若多次触发，重新计算时间
```javascript
            function debounce(fn,wait) {
                let timer = null;
                return function () {
                    clearTimeout(timer)
                    timer = setTimeout(()=>{
                        fn.apply(this,arguments)
                    },wait)
                }
            }
 ```

 ### Vue
1. vuex是一个专为vue.js应用程序开发的状态管理器，它采用集中式存储管理应用的所有组件的状态，并且以相
应的规则保证状态以一种可以预测的方式发生变化。

state: vuex使用单一状态树，用一个对象就包含来全部的应用层级状态

mutation: 更改vuex中state的状态的唯一方法就是提交mutation

action: action提交的是mutation，而不是直接变更状态，action可以包含任意异步操作

getter: 相当于vue中的computed计算属性

2. Vue是采用数据劫持配合发布者-订阅者模式，通过Object.defineProperty来()来劫持各个属性的getter和setter
在数据发生变化的时候，发布消息给依赖收集器，去通知观察者，做出对应的回调函数去更新视图。

具体就是：
MVVM作为绑定的入口，整合Observe,Compil和Watcher三者，通过Observe来监听model的变化
通过Compil来解析编译模版指令，最终利用Watcher搭起Observe和Compil之前的通信桥梁
从而达到数据变化 => 更新视图，视图交互变化(input) => 数据model变更的双向绑定效果。
### css3
1.过渡 transition
2.动画 animation
3.形状转换 transform
4.阴影 box-shadow
5.滤镜 Filter
6.颜色 rgba
7.栅格布局 gird
8.弹性布局 flex
### computed 与 watch区别
①从属性名上，computed是计算属性，也就是依赖其它的属性计算所得出最后的值。watch是去监听一个值的变化，然后执行相对应的函数。
②从实现上，computed的值在getter执行后是会缓存的，只有在它依赖的属性值改变之后，下一次获取computed的值时才会重新调用对应的getter来计算。watch在每次监听的值变化时，都会执行回调。其实从这一点来看，都是在依赖的值变化之后，去执行回调。很多功能本来就很多属性都可以用，只不过有更适合的。如果一个值依赖多个属性（多对一），用computed肯定是更加方便的。如果一个值变化后会引起一系列操作，或者一个值变化会引起一系列值的变化（一对多），用watch更加方便一些。
③watch的回调里面会传入监听属性的新旧值，通过这两个值可以做一些特定的操作。computed通常就是简单的计算。
④watch和computed并没有哪个更底层，watch内部调用的是vm.$watch，它们的共同之处就是每个定义的属性都单独建立了一个Watcher对象。

                         
            
              
        
              