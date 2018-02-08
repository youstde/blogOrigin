---
title: Vue学习笔记-双向绑定实现原理
date: 2017-03-01 22:56:23
tags:
---

本文是转载的博文，觉得总结的挺好的，就直接搬过来了，[**原文走这里**](http://www.tuicool.com/articles/MZ73i2m)

### 一、实现双向绑定的做法

前端MVVM最令人激动的就是双向绑定机制了，实现双向数据绑定的做法大致如下三种：

#### 1.发布者-订阅者模式（backboneJS）

思路：使用自定义的data属性在HTML代码中指明绑定。所有绑定起来的JavaScript对象以及DOM元素都将“订阅”一个发布者对象。任何时候如果JavaScript对象或者一个HTML输入字段被侦测到发生了变化，我们将代理事件到发布者-订阅者模式，这会反过来讲变化广播并传播到所有绑定的对象元素。

#### 2.脏值检查（angularJS）

思路：angular.js是通过脏值检测的方式比对数据是否有变更，从而来决定是否更新视图。最简单的方式就是通过setInterval()定时轮询检测数据是否发生变化，Angular只在特定的事件触发时进行脏值检测，大致如下：

* DOM事件，譬如用户输入文本，点击按钮等(ng-click)
* XHR响应事件($http)
* 浏览器Location变更事件($location)
* Timer事件($timeout, $interval)
* 执行$digest()或$apply()

#### 3.数据劫持（VueJS）

思路：vue.js则是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter， 在数据变动时发布消息给订阅者，触发响应的监听回调。

由此可见，Object.defineProperty()这个API是Vue实现双向绑定的关键，[**了解更多走这里**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

### 二、Object.defineProperty()

简单例子：

```javascript
    var obj = {};
    Object.defineProperty(obj, 'hello', {
        get: function() {
            console.log('get val:'+ val);
            return val;
     　 },
    　　set: function(newVal) {
            val = newVal;
            console.log('set val:'+ val);
        }
    });
    obj.hello;
    obj.hello='111';

```

结果：

![](http://ww1.sinaimg.cn/large/005QDhBjgy1fd7roq2nzqj30qo04qglq)

如果去掉 obj.hello=‘111’ 这行代码，则get的返回值val会报错val is not 
defined。可见Object.defineProperty() 
监控对数据的操作，可以自动触发数据同步。下面我们先用Object.defineProperty()来实现一个非常简单的双向绑定。

### 三、实现简单的双向绑定

```html
<!DOCTYPE html>
 <head></head>
 <body>
  <div id="app">
    <input type="text" id="a">
    <span id="b"></span>
  </div>

  <script type="text/javascript">
   var obj = {};
   Object.defineProperty(obj, 'hello', {
       get: function() {
           console.log('get val:'+ val);
           return val;
       },
       set: function(newVal) {
            val = newVal;
            console.log('set val:'+ val);
            document.getElementById('a').value = val;
            document.getElementById('b').innerHTML = val;
       }
    });
    document.addEventListener('keyup', function(e) {
      obj.hello = e.target.value;
    });
   </script>
  </body>
</html>
```

演示效果：

![](http://ww1.sinaimg.cn/large/005QDhBjgy1fd7rsh89gig30nm0ajte1)

