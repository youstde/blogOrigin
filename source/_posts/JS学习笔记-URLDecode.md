---
title: JS学习笔记-URLDecode
date: 2017-04-11 22:07:46
tags:
---

最近工作中遇到了一个问题：

请求地址：**http://example.com/index.html?data={pid:17_29_31,materialId:5}&appKey=59B46E3C4218B4055474599B7F87379B&adTag=#/nineSquare**

请求一个页面的时候url中拼了一串参数，其中有一些特殊字符，而我要做的是将这些参数取下来，然后拼入新的参数再传给后端

一开始我就直接通过分割字符串的方式取中间的数据，OK，没有问题，成功返回给后端

但是在我用手机去测的时候发现参数取不到，最后发现是url中特殊字符被转义了

由于一开始对于**decodeURIComponent（）**这个方法有点陌生，所以没有想起来，然后继续去做兼容性的分割字符串，后来发现这样写代码很冗余，于是上网搜了一下才想起来用**decodeURIComponent()**

首先获取到**window.location.search()**中的字符串后，将其decode一下，然后如何符合JSON格式就转成JSON格式的取一下数据，如果不满足就直接分割字符串

到此，这个问题算是解决了！