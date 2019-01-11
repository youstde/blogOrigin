---
title: AngularJS
date: 2017-03-01 21:31:21
tags:
---



### 1.$apply()方法的使用

场景：

当前页面用户是登录状态，而后端想用一个新的用户数据去冲掉之前的用户登录，这个时候最好的方式是使用这个$apply()方法

## html

```html
<div ng:app ng-controller="Ctrl">{{message}}</div>
```

## JS

```javascript

functionCtrl($scope) {
  $scope.message ="Waiting 2000ms for update";   
  setTimeout(function () {
  　　$scope.message ="Timeout called!";
     // AngularJS unaware of update to $scope
  }, 2000);
}
```



上面的代码执行后页面上会显示：Waiting 2000ms for update，显然，数据的更新并没有被Angular捕捉到

此时将代码改一下，就可以成功

## JS

```javascript
functionCtrl($scope) {
  $scope.message ="Waiting 2000ms for update";
  setTimeout(function () {
  　　$scope.$apply(function () {
     　　$scope.message ="Timeout called!";
      });
  }, 2000);
}
```

这次与之前不同的是，页面上先会显示：Waiting 2000ms for update，等待2秒后内容会被更改为：Timeout called! 。显然数据的更新被angular JS觉察到了。

     NOTE：我们不应该这样做，而是用angular JS提供的timeout方法，这样它就会被自动用apply方法包起来了。