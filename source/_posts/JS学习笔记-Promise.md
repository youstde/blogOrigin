---
title: JS学习笔记-Promise
date: 2018-05-09 15:19:12
tags:
---

此文是最近又温习了一下Promise的源码，发现自己又懵逼了，于是决定在我被虐完还清醒的状态下记点什么，免得过段时间再看又是那个状态。。

关于Promise源码解读比较好的有篇文章不错，可以[传送](https://tech.meituan.com/promise-insight.html)过去看一下

借鉴他文章里的一个🌰：

通过用户id获取其手机号

```javascript
//获取用户ID
function getUserId() {
    return new Promise(function (resolve) {
        resolve(9876);
    });
}

//通过ID获取手机号
function getUserMobileById(id) {
    return new Promise(function (resolve) {
        Y.io('/usermobile/' + id, {
            on: {
                success: function (i, o) {
                    resolve(JSON.parse(o).mobile);
                }
            }
           });
    });
}
```

此时想做的就是getUserId获取到ID后传给getUserMobileById去换取手机号，于是调用的方式就是这样的：

```javascript
//例1
getUserId()
    .then(getUserMobileById)
    .then(function (mobile) {
        // do sth else with mobile
    }, function (error) {
        // getUserId或者getUerMobileById时出现的错误
        console.log(error);
    });
```

当然也可以有另外的写法

```javascript
//例2
function getUserId() {
    return new Promise(function (resolve) {
        resolve(getUserMobileById(9876));
    });
}
getUserId().then(function(res){
    console.log(res);
})
```

其实两种写法原理都是一样的，只是写的位置不同而已，今天要说的是第一种写法。

先放几个关键的方法：

```javascript
this.then = function (onFulfilled) {
    return new Promise(function (resolve) {
        handle({
            onFulfilled: onFulfilled || null,
            resolve: resolve
        });
    });
};

function handle(deferred) {
    if (state === 'pending') {
        deferreds.push(deferred);
        return;
    }

    var ret = deferred.onFulfilled(value);
    deferred.resolve(ret);
}

function resolve(newValue) {
    if (newValue && (typeof newValue === 'object' || typeof newValue === 'function')) {
        var then = newValue.then;
        if (typeof then === 'function') {
            then.call(newValue, resolve);
            return;
        }
    }
    state = 'fulfilled';
    value = newValue;
    setTimeout(function () {
        deferreds.forEach(function (deferred) {
            handle(deferred);
        });
    }, 0);
}
```

回到例1

```javascript
getUserId()
    .then(getUserMobileById)
    .then(function (mobile) {
        // do sth else with mobile
    }, function (error) {
        // getUserId或者getUerMobileById时出现的错误
        console.log(error);
    });
```

1.首先执行getUserId，其返回的是一个Promise实例（IDPro），执行其内部的逻辑，调用IDPro的resolve方法并传入9876

2.resolve将IDPro的pending变成fulfilled状态，延迟遍历deferreds数组，同时循环执行handle并将deferreds的每项传入进去

3.进入handle方法中，此时的IDPro状态为fulfilled，所以他会执行传入的deferred中的onFulfilled和resolve，此时，onFulfilled为getUserMobileById（MOPro），resolve属于第一次调用then方法生成的bridge Promise。执行onFulfilled将value（9876）传入，返回值是一个Promise实例，执行其内部的逻辑，然后将该实例传入resolve中。

4.进入resolve中后，由于满足上面的条件，所以直接调用了MOPro的then方法，将bridge Promise的resolve传入进去并返回。

5.当MOPro执行了其内部的resolve时，将MOPro的状态变成了fulfilled，延迟遍历deferreds数组，同时循环执行handle并将deferreds的每项传入进去

6.进入handle方法中，此时onFulfilled为IDPro的then方法返回的bridge Promise的resolve，调用onFulfilled将value传入，此时bridge Promise会执行其then方法中注入的deferred，其实它的then方法就是例1中的第二个then方法，它不是MOPro的then方法，而是bridge Promise。。。

回头再去看这张也就觉得合理了

![](https://tech.meituan.com/img/promise-series-flow.png)







