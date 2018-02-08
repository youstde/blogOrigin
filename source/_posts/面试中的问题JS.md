---
title: 面试中的问题JS
date: 2017-11-28 10:26:00
tags:
---

## `window.onload和$(document).ready()的区别`

1.执行时间

- window.onload会等到页面所有元素加载完成才会执行
- $(document).ready()是DOM结构绘制完毕后就执行

2.编写个数不同

- window.onload不能同时编写多个，如果有多个window.onload方法，只会执行一个 
- $(document).ready()可以同时编写多个，并且都可以得到执行 

#### WHY

1.执行时间上：

$(document).ready()监听的是`DOMContentLoaded`，所以DOM结构绘制完成后就执行

2.编写个数

jQuery是通过addEventListener去监听



## `原型和原型链`



![](http://ww1.sinaimg.cn/large/005QDhBjgy1flxlhdjq0nj30fu0jpmyk.jpg)

其实，只需要明白原型对象的结构即可：

```javascript
    Function.prototype = {
        constructor : Function,
        __proto__ : parent prototype,
        some prototype properties: ...
    };
```

总结：`函数的原型对象constructor默认指向函数本身，原型对象除了有原型属性外，为了实现继承，还有一个原型链指针__proto__，该指针指向上一层的原型对象，而上一层的原型对象的结构依然类似，这样利用__proto__一直指向Object的原型对象上，而Object的原型对象用Object.prototype.__proto__ = null表示原型链的最顶端，如此变形成了javascript的原型链继承，同时也解释了为什么所有的javascript对象都具有Object的基本方法。`