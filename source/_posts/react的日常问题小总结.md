---
title: react的日常问题小总结
date: 2018-02-08 14:13:36
tags:

---

## `写在前面`

先附上一些关于react的文章，留着以后查漏补缺用：

[react的生命周期](https://www.jianshu.com/p/4784216b8194)

[对react一些原理性的理解](https://segmentfault.com/a/1190000011523084)

* React合成事件系统

[react-router的实现原理](https://segmentfault.com/a/1190000004527878)

[React中setState同步更新策略](https://segmentfault.com/a/1190000008051628)

##### 1.为什么在componentWillUpdate中调用setState会造成循环调用

```javascript
如果在shouldComponentUpdate和componentWillUpdate中调用了setState，此时this._pendingStateQueue != null，则performUpdateIfNecessary方法就会调用updateComponent方法进行组件更新。但是updateComponent方法又会调用shouldComponentUpdate和componentWillUpdate，因此造成循环调用，使得浏览器内存占满后崩溃。
```

#####2.componentDidMount里调用setState会触发componentWillUpdate，但是拿不到新设置的state

```
setState是一个不保证同步的过程
```

一般设置state都会放在componentWillMount中去做，这个时候触发componentWillUpdate的函数中就可以得到改变的state



