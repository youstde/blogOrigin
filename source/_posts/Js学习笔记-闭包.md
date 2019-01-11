---
title: Js学习笔记-闭包
date: 2017-03-04 23:25:01
tags:
---

### 优点：

1.缓存

2.实现封装，防止变量跑到外层作用域中，发生命名冲突

### 缺点：

1.内存消耗

2.性能问题

​	使用闭包时，会涉及到跨作用域访问，每次访问都会导致性能损失。

### 解决内存泄漏

原始代码：

```javascript

function Cars(){
  this.name = "Benz";
  this.color = ["white","black"];
}
Cars.prototype.sayColor = function(){
  var outer = this;
  return function(){
    return outer.color
  };
};
 
var instance = new Cars();
console.log(instance.sayColor()())
```

优化后的代码：

```javascript
function Cars(){
  this.name = "Benz";
  this.color = ["white","black"];
}
Cars.prototype.sayColor = function(){
  var outerColor = this.color; //保存一个副本到变量中
  return function(){
    return outerColor; //应用这个副本
  };
  outColor = null; //释放内存
};
 
var instance = new Cars();
console.log(instance.sayColor()())
```

稍微复杂一点例子：

```javascript
function inheritPrototype(subType,superType){
  var prototype = Object(superType.prototype);
  prototype.constructor = subType;
  subType.prototype = prototype;
}
 
function Cars(){
  this.name = "Benz";
  this.color = ["white","black"];
}
Cars.prototype.sayColor = function(){
  var outer = this;
  return function(){
    return outer.color;
  };
};
 
function Car(){
  Cars.call(this);
  this.number = [321,32];
}
inheritPrototype(Car,Cars);
Car.prototype.sayNumber = function(){
  var outer = this;
  return function(){
    return function(){
      return outer.number[outer.number.length - 1];
    }
  };
};
 
var instance = new Car();
console.log(instance.sayNumber()()());
```

