---
title: CSS学习笔记-实现滚动进度条效果
date: 2019-01-11 15:03:45
tags:
---

#### :fallen_leaf:准备

在切入正题之前，先看两个与之相关的知识点：

#####  :candy: calc()

> 用于动态计算长度值
>
> 支持的版本：css3

需要注意的是：

* 运算符前后都要保留一个空格，例如：

  ```css
  width: calc(100% - 10px);
  ```

* 任何长度都可以使用calc()函数进行计算

* calc()支持“+”，“-”，“*”，“/”运算

* calc()函数遵循数学运算优先级规则

##### :candy: vh

> 相对于视口的高度
>
> 视口被均分为100个单位的vh

例如：

```css
.bw {
    font-size: 16vh;
}
```

如果此时的视口高度为100px，那么上述的示例中的字体大小为(16/100) * 100,既为16px

#### :fallen_leaf: JS实现形式

![转载](http://bwblog.oss-cn-hangzhou.aliyuncs.com/blogimg/scroll-indicator-2.gif)

如图，可以看到滚动提示条的实现是通过控制`scroll-line`的宽度。也就是在滚动内容的时候，width的值从0%变化到100%，而要实现这一点，就得知道两个参数：

* 文档的高度 `document.body.scrollHeight || $(document).height()`
* 可视区域的高度 `document.body.clientHeight || $(window).height() `

如下图所示：

![转载](http://bwblog.oss-cn-hangzhou.aliyuncs.com/blogimg/scroll-indicator-3.png)

文档高度和视口高度有一个差值max，而这个值是需要滚动的值，简单理解就是`scroll-line`宽度为100%时的值。这样我们就知道了，整个文档滚动的最大值。另外，我们还需要知道滚动条滚动的距离$(window).scrollTop(),有了这两个值，我们就可以算出进度条当前的宽度`($(window).scrollTop() / ($(document).height() - $(window).height())) * 100%`

![转载](http://bwblog.oss-cn-hangzhou.aliyuncs.com/blogimg/scroll-indicator-4.gif)

#### 🍂 CSS实现形式

首先，为了实现进度提示条的功能得用到`线性渐变`

[原文地址](https://juejin.im/post/5c35953ce51d45523f04b6d2?utm_source=gold_browser_extension)

[demo](https://codepen.io/youstde/pen/PXyaYQ)

#### 🍂 拓展

**1、要获取当前页面的滚动条的纵坐标的位置，应该用：**​      

```
document.documentElement.scrollTop
```

之所以不用document.body.scrollTop的原因是documentElement对应的是html标签,而body对应的是body标签

**2、关于document.body和document.documentElement的区别**

body是DOM对象中body的子节点,即<body>标签，而documentElement是整个节点树的根结点，即<html>标签。
即document.body.clientHeight获取到的是body的高度，而document.documentElement.clientHeight获取到的是   整个html的高度，然而当文档处于怪癖模式下时，我们使用document.documentElement.scrollTop获取到属性的值是为0的，而在w3c标准文档模式下，document.body.scrollTop的值是为0的，因此为了好的兼容性，我们可以这样写：

**var top = document.documentElement.scrollHeight || document.body.scrollHeight**

