---
title: 移动端-webview
date: 2017-05-03 10:11:42
tags:
---

## webview

### 1.什么是webview

如果我们想把h5的页面嵌入到原生的app中去使用，就需要一个容器去运行和展示我们的页面，webview就是这种容器中的一种

### 2.问题

在安卓手机中**5.0版本及以上**会出现在https的页面中无法请求http的资源

### 3.解决办法

1.把所有的资源链接的http头去掉，以斜线开头（**前提是资源是支持https的**）

```javascript
//youstde.github.io/
```

2.其他方法正在搜寻中。。。

