---
title: JS学习笔记-Promise链式调用解析
date: 2018-11-13 14:24:08
tags:
---

## Promise链式调用解析

关于Promise是做什么的我就不赘述了，当你看到这个文章的时候，我也就默认你是用过Promise的

首先，举个🌰：

```javascript
// 获取用户id
function getUserId() {
    return new Promise(function(resolve, reject) {
      setTimeout(function() {
        resolve(186)
      }, 1e3)
    })
}
// 通过用户id获取该用户的手机号
function getMobileByUserId(userId) {
    return new Promise(function(resolve, reject) {
        setTimeout(function(){
            resolve(userId + '****5836')
        }, 1e3)
    })
}
```

两个方法，getUserId(简称**IDfun**)和getMobileByUserId(简称**MOfun**),两者都是return了一个Promise实例，前者是获取用户id，后者是拿着用户id去换取手机号，MOfun的输出是依赖于IDfun的输出，这样两个异步的操作又得保证其是有序的执行

到这，应该都有一个对应的解决办法，我这里列举了一个示例：

```javascript
IDfun()
    .then(MOfun)
    .then(function(res) {
    	console.log(res)
	})
```

首先，调用了IDfun，在其Promise实例的then方法中传入MOfun，紧随其后又是一个then方法，里面传入了一个callback函数，此时，callback函数中打印的值是什么，又是为什么呢，一般情况下then方法里传的都是一个callback，而这个例子里传的却是一个包含Promise的函数，它内部是怎么做到将用户的id传递给MOfun中并打印出来

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

说到这，我先列举几个Promise中几个比较核心的方法：

```javascript
// this指向是Promise实例
// 只列举了几个关键的代码
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
// 其中的state和value可以理解为全局定义的，只是放了一些代码段出来
function resolve(newValue) {
    if (newValue && (typeof newValue === 'object' || typeof newValue === 'function')) {
        var then = newValue.then;
        if (typeof then === 'function') {
            then.call(newValue, resolve);
            return;
        }
    }
    state = 'fulfilled';
    value = newValue;     // value这里可以理解为它是在全局定义的
    setTimeout(function () {
        deferreds.forEach(function (deferred) {
            handle(deferred);
        });
    }, 0);
}
```

参照这几个方法，我们再回到刚才那个例子：

- 1.执行IDfun返回一个Promise实例（**IDPro** ），执行IDPro中的代码，假设此时在异步发送请求，继续执行then方法，then方法中传入了MOfun
- 2.进入到then方法中，也是返回一个Promise实例（**BridgePro1**）,调用handle方法

```javascript
        handle({
            onFulfilled: onFulfilled || null,  // 此时的onFulfilled === MOfun
            resolve: resolve  // resolve是BridgePro1的resolve
        });
```

- 3.进入handle中，如果是IDPro的then方法先执行，其resolve后执行，这个时候 `state` === `pending`,此时它会将传入的对象push到IDPro的deferreds数组中，然后返回
- 4.IDPro中异步操作完成，执行其resolve，并传入id为186
- 5.进入resolve方法中，此时newValue不满足if条件，跳过，继续向下执行，改变`state`===> `fulfilled`,`value` ===> `186`,setTimeout为0是为了把其内部的这段代码放到队列的最后，保证执行这段代码的时候then方法已经执行了,循环遍历IDPro的deferreds数组，将数组中的每一项传入handle中并执行

```javascript
function resolve(newValue) {   // newValue === 186
    if (newValue && (typeof newValue === 'object' || typeof newValue === 'function')) {
        var then = newValue.then;
        if (typeof then === 'function') {
            then.call(newValue, resolve);
            return;
        }
    }
    state = 'fulfilled';
    value = newValue;     // value这里可以理解为它是在全局定义的
    setTimeout(function () {
        // deferreds里存放的是通过then传入的
        // [{
        //    onFulfilled: MOfun,
        //    resolve: resolve
        // }]
        deferreds.forEach(function (deferred) {  
            handle(deferred);
        });
    }, 0);
}
```

- 6.进入handle中，此时state === fulfilled，执行deferred中的onFulfilled（MOfun）,传入value,返回一个Promise实例（**MOPro**）,此时的ret === MOPro，继续执行deferred中的resolve（BridgePro1的resolve），传入ret

```javascript
// state === fulfilled
// deferred ===> 
//  {
//    onFulfilled: MOfun,
//    resolve: resolve
//  }
function handle(deferred) {
    if (state === 'pending') {
        deferreds.push(deferred);
        return;
    }

    var ret = deferred.onFulfilled(value);  // value === 186
    deferred.resolve(ret);
}
```

- 7.进入resolve中,此时newValue满足判断条件

```javascript
function resolve(newValue) {   // newValue === MOPro
    if (newValue && (typeof newValue === 'object' || typeof newValue === 'function')) {
        // then 是MOPro的then
        var then = newValue.then;
        if (typeof then === 'function') {
            // 调用then方法，设置其内部this指向为MOPro，并传入resolve，这个resolve为BridgePro1的resolve，然后返回
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

- 8.进入到MOPro的then方法中，调用handle将对象传入，此时MOPro的deferreds数组中有两项，一项是上面通过resolve传入的，另一项是传入的callback函数

```javascript
// ===> callback
function(res) {
    console.log(res)
}
```

```javascript
this.then = function (onFulfilled) {
    // onFulfilled为BridgePro1的resolve 
    // 此时返回的Promise实例为BridgePro2
    return new Promise(function (resolve) {
        handle({
            onFulfilled: onFulfilled || null,
            resolve: resolve  // BridgePro2
        });
    });
};
```

- 9.当MOPro中异步操作执行完成，执行resolve并传入手机号`1865836`，进入resolve,将state设置为fulfilled，value设置成`1865836`，延迟循环MOPro的deferreds数组，此时数组为：

```javascript
[
    {
        onFulfilled: resolve,   // BridgePro1的resolve
        resolve: resolve  // BridgePro2的resolve
    }，
    {
     	onFulfilled: callback,
        resolve: resolve  // BridgePro3的resolve
    }
]
```

- 10.进入handle方法中,循环第一个值时，此时deferred的onFulfilled为BridgePro1的resolve，调用该resolve，并传入value（`1865836`）,因为BridgePro1的deferreds为空，所以直接resolve掉BridgePro1，此时ret为undefined，再执行deferred的resolve（BridgePro2的resolve），同样，resolve掉BridgePro2，继续循环，传入第二个值，deferred的onFulfilled为callback，执行callback传入value，打印出手机号`1865836`,返回值为undefined ,因此ret为undefined，再执行deferred的resolve（BridgePro3的resolve），直接resolve掉BridgePro3

```javascript
function handle(deferred) {
    if (state === 'pending') {
        deferreds.push(deferred);
        return;
    }

    var ret = deferred.onFulfilled(value);
    deferred.resolve(ret);
}
```

- 11.至此，执行完成，通过user的id换取了user的mobile number

#### 后记：

如果我想按照下面的写法一直then下去：

```javascript
IDfun()
    .then(MOfun)
    .then(callback)
	.then(callback)
	.then(callback)
```

每个callback里都可以打印到手机号，应该怎么写

```javascript
// callback
function callback(res) {
    // doing something
    console.log(res)
    return res
}
```

#### 总结：

Promise是什么，从字面意思就是一个承诺，我给你了一个承诺，你记着呢，不知道什么时候兑现，但肯定会给你一个答复，打一个不太形象的比喻，我去银行办事，肯定是先取一个号，这个号就像一个承诺，什么时候叫到你，不确定，但肯定会叫你，然后这个时候你就会去办理你的事情

最后附上一个流程图（图片来源于美团）：

![](http://ww1.sinaimg.cn/large/005QDhBjly1fx6ewjrnfhj30j90cdwfk.jpg)