---
title: ' jQuery 核心函数'
date: 2020-11-23 19:43:25
tags: [jQuery,Javascript]
published: true
hideInList: false
feature: https://tva3.sinaimg.cn/large/0072Vf1pgy1foxkcwa5wxj31hc0u04hn.jpg
isTop: false
---

今天主要分析的特点：

1. 无new来构建实例对象
2. 共享原型的设计
3. extend 方法 ，为自身以及实例对象添加方法， 以及进行对象的浅拷贝与深拷贝
 
##  1. 无new构建jQuery实例对象 

jQuery 通过将jQ函数挂载到全局对象window上，并通过简写 $ 符来进行调用，在 js 产生一个实例对象需要调用 new 操作符来进行生成实例对象,所以可以通过返回一个构造函数的调用来实现无 new 构建实例，如下

```javascript
(function (root) {
                
    function jQuery() {
        /**
         *  形成死循环  
         */
        return new jQuery()  
    }

    root.jQuery = root.$ = jQuery
})(this)

 ```

但下面这种方法会出现无限循环，因此 jQuery 对其进行了优化改进，通过修改 jQuery 函数的原型添加一个 init 属性来实现

```javascript
(function (root) {
                
    function jQuery() {
        /**
         *  形成死循环  
         */
        return jQuery.fn.init()  
    }

        /**
     *  重写jQuery 原型，来实现 构建无new操作符实例 ， 将jQuery的方法放在原型上
     */
    jQuery.prototype = {
        /**
         *  构建实例对象
         */
        init: function () {

        },
    }

    /**
    *  简写原型 
    */

    jQuery.fn = jQuery.prototype

    jQuery.fn.init.prototype = jQuery.fn;

    root.jQuery = root.$ = jQuery

})(this)

```

将 init 函数的原型绑定到 jQuery 函数的原型上， 来实现原型共享，同时也达到 调用构造函数生成实例的目的

## 2. extend 方法实现

1. 首先分析 jQuery.extend 方法具有哪些功能
    1. 当调用 $.extend() 方法传入一个对象时， jQuery 会将该对象的属性合并到 jQuery 自身上去
    2. 当调用 $.fn.extend() 方法传入一个对象时， 会将该对象的属性合并到 jQuery 实例身上去
    3. 当 $.extend() 传入多个参数时，且第一个参数为 Boolean 格式时，为深拷贝 ， 否则第一个参数必为对象

首先无论是 jQuery 还是 jQuery 的实例，都会有 extend 方法；因此可以先创建一个 extend 函数挂到 jQuery 及其原型上

```javascript
jQuery.extend = jQuery.fn.extend = function () {}  
```

然后就是对传入的参数进行判断处理了，根据上面的需求，对参数的个数以及首位参数的类型进行验证，判断是向 jQuery 添加属性方法还是仅仅是用来做合并对象的操作 ，代码如下 （）

```javascript

 jQuery.extend = jQuery.fn.extend = function () {
      
    var target = arguments[0] || {};
    var length = arguments.length;
    var deep = false
    var i = 1;  //  记录 循环的起始位置，避免造成不必要的循环。


    if (jQuery.isBoolean(target)) {
        deep = true;
        target = arguments[1] || {};
        i++
    }
    /**
     *  如果target 不是复杂数据类型 ， 就要将其转为 {}；
     */
    if(typeof target !== 'object'){
        target = {};
    }

    /**
      *  如果仅有一个参数，代表着向 jQuery 或者 jQuery 实例上去添加属性方法
      */  
    if (i === length) {
        target = this;
        i--
    }

    for (; i < length; i++) {
        var option = arguments[i];
        for (var name in option) {
          target[name] = option[name]
        }
    }
    return target

}

```

当进行深拷贝时，就需要额外的添加一些判断逻辑， 来验证待拷贝的目标是否是复杂数据类型 以及与拷贝的目标的数据格式是否统一，如果格式不一致就要修改拷贝目标的格式了。完整代码如下

```javascript

(function(root) {
    /**
     *  jQuery 特点 
     * 
     * 1. 无new 构建实例对象
     * 
     * 2. 共享原型
     * 
     * 3. extend 拓展
     * 
     */
    function jQuery() {
        /**
         *  形成死循环  
         */
        // return new jQuery()  
        return new jQuery.prototype.init()
    }
    /**
     *  重写jQuery 原型，  来实现 构建无new操作符实例 ， 将jQuery的方法放在原型上
     */
    jQuery.prototype = {
            /**
             *  构建实例对象
             */
            init: function() {

            },
            html: function() {

            },
            css: function() {

                }
                //  ...
        }
        /**
         *  简写原型 
         */
    jQuery.fn = jQuery.prototype
        /**
         *  实现原型 共享 原型
         */
    jQuery.fn.init.prototype = jQuery.fn;
    /**
     *  extend 
     * 
     * 1. 可以给任意多个对象合并
     * 
     * 2. 可以拓展jQuery本身
     * 
     * 3. 可以拓展jQuery实例方法
     */
    jQuery.extend = jQuery.fn.extend = function() {
        /**
         * 参数分为多种情况 
         * 
         * 1. 有多个参数时 
         *      1. 第一个参数为 boolean 且为true 时  则代表要进行深拷贝
         *      
         *      2. 否则第一个参数必为对象
         * 
         * 2. 只有一个参数时
         *      
         *      1. 代表着给jQuery本身 / jQuery 实例 添加拓展方法
         * 
         */
        var target = arguments[0];
        var length = arguments.length;
        var deep = false
        var i = 1;

        if (Object.prototype.toString.call(target).slice(8, -1) === 'Array') {
            deep = true;
            target = arguments[1]
            i++
        }
        if (typeof target != 'object') {
            target = {}
        }
        if (i === length) {
            target = this;
            i--
        }
        for (; i < length; i++) {
            var option = arguments[i];
            for (var name in option) {
                var copy = option[name];
                var src = target[name];
                var isArray = Object.prototype.toString.call(copy).slice(8, -1) === 'Array';
                var isObject = Object.prototype.toString.call(copy).slice(8, -1) === 'Object'
                var clone = {};
                if (deep && (isArray || isObject)) {
                    if (isArray) {
                        clone = Object.prototype.toString.call(src).slice(8, -1) === 'Array' ? src : [];
                    } else if (isObject) {
                        clone = Object.prototype.toString.call(src).slice(8, -1) === 'Object' ? src : {}
                    } else {
                        clone = src
                    }
                    target[name] = jQuery.extend(deep, clone, copy)
                } else if (copy !== undefined) {
                    target[name] = copy
                }
            }
        }

        return target
    }
    jQuery.extend({
        isPlainObject: function(data) {
            return Object.prototype.toString.call(data).slice(8, -1) === 'Object'
        },
        isFunction: function(fn) {
            return Object.prototype.toString.call(fn).slice(8, -1) === 'Function'
        },
        isArray: function(data) {
            return Object.prototype.toString.call(data).slice(8, -1) === 'Array'
        },
        isBoolean: function(data) {
            return Object.prototype.toString.call(data).slice(8, -1) === 'Boolean'
        },
    })


    //  将jQuery挂载到window（全局变量）上
    root.jQuery = root.$ = jQuery
})(this)

```










