---
title: JS学习笔记-函数节流与函数去抖
date: 2017-03-02 23:29:21
tags:
---

### 函数节流(throttle)

**1. 定义**

　　如果将水龙头拧紧直到水是以水滴的形式流出，那你会发现每隔一段时间，就会有一滴水流出。

　　也就是会说预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期。

简单实现：

```javascript
var throttle = function(delay, action){
  var last = 0return function(){
    var curr = +new Date()
    if (curr - last > delay){
      action.apply(this, arguments)
      last = curr 
    }
  }
}
```

### 函数去抖(debounce)

**1. 定义**

　　如果用手指一直按住一个弹簧，它将不会弹起直到你松手为止。

       也就是说当调用动作n毫秒后，才会执行该动作，若在这n毫秒内又调用此动作则将重新计算执行时间。

简单实现：

```javascript
var debounce = function(idle, action){
  var last
  return function(){
    var ctx = this, args = arguments
    clearTimeout(last)
    last = setTimeout(function(){
        action.apply(ctx, args)
    }, idle)
  }
}
```



[原文点这里](http://www.cnblogs.com/fsjohnhuang/p/4147810.html)

