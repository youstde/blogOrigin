---
title: CSS学习笔记-滚动视差
date: 2019-01-14 10:37:18
tags:
---

![转载](https://user-gold-cdn.xitu.io/2018/8/10/16521e61a172b7d3?imageslim)

如图，在页面滚动时，要实现这样的效果应该怎么做？

### 准备

<code>background-attachment</code>

> 它主要是解决如何设置固定的背景图像
>
> 可以点击[**w3c**](http://www.w3school.com.cn/cssref/pr_background-attachment.asp)，查看详细说明

`background-attachment: scroll`

**scroll** 此关键字表示背景相对于元素本身固定， 而不是随着它的内容滚动。

`background-attachment: local`

**local** 此关键字表示背景相对于元素的内容固定。如果一个元素拥有滚动机制，背景将会随着元素的内容滚动， 并且背景的绘制区域和定位区域是相对于可滚动的区域而不是包含他们的边框。

`background-attachment: fixed`

**fixed** 此关键字表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。

> 注意一下 scroll 与 fixed，一个是相对元素本身固定，一个是相对视口固定，有点类似 `position` 定位的 `absolute` 和 `fixed`。

## 使用 `background-attachment: fixed` 实现滚动视差

首先，我们使用 `background-attachment: fixed` 来实现滚动视差。**fixed** 此关键字表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。

这里的关键在于，即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动。也就是说，背景图从一开始就已经被固定死在初始所在的位置。

我们使用，图文混合排布的方式，实现滚动视差，HTML 结构如下，`.g-word` 表示内容结构，`.g-img` 表示背景图片结构：

```
<section class="g-word">Header</section>
<section class="g-img">IMG1</section>
<section class="g-word">Content1</section>
<section class="g-img">IMG2</section>
<section class="g-word">Content2</section>
<section class="g-img">IMG3</section>
<section class="g-word">Footer</section>
复制代码
```

关键 CSS：

```
section {
    height: 100vh;
}

.g-img {
    background-image: url(...);
    background-attachment: fixed;
    background-size: cover;
    background-position: center center;
}
```

效果如下：

![转载](https://user-gold-cdn.xitu.io/2018/8/10/16521e893d1a43ee?imageslim)

### `background-attachment: fixed` 实现图片点击水纹效果

利用图片相对视口固定，可以有很多有趣的效果，譬如下面这个，来源于这篇文章[CSS Water Wave (水波效果)](https://www.oxxostudio.tw/articles/201407/css-water-wave.html)：

![转载](http://bwblog.oss-cn-hangzhou.aliyuncs.com/blogimg/20140729_1_03.gif)

HTML:

```html
<div class="wave wave5"></div>
<div class="wave wave4"></div>
<div class="wave wave3"></div>
<div class="wave wave2"></div>
<div class="wave wave1"></div>
<div class="wave wave0"></div>
```

CSS:

```scss
.wave{
  position:absolute;
  top:calc((100% - 30px)/2);
  left:calc((100% - 30px)/2);
  width:30px;
  height:30px;
  border-radius:300px;
}
.wave0{
  background:#f00;
  z-index:2;
  background-size:auto 106%;
  -webkit-animation:w 1s forwards;
}
.wave1{
  background:#d00;
  z-index:3;
  -webkit-animation:w 1s .2s forwards;
}
.wave2{
  background:#b00;
  z-index:4;
  -webkit-animation:w 1s .4s forwards;
}
.wave3{
  background:#900;
  z-index:5;
  -webkit-animation:w 1s .5s forwards;
}
.wave4{
  background:#700;
  z-index:6;
  -webkit-animation:w 1s .8s forwards;
}
.wave5{
  background:#500;
  z-index:7;
  -webkit-animation:w 1s 1s forwards;
}
@-webkit-keyframes w{
  0%{
    top:calc((100% - 30px)/2);
    left:calc((100% - 30px)/2);
    width:30px;
    height:30px;
  }
  100%{
    top:calc((100% - 300px)/2);
    left:calc((100% - 300px)/2);
    width:300px;
    height:300px;
  }
}
```

效果：

![转载](http://bwblog.oss-cn-hangzhou.aliyuncs.com/blogimg/20140729_1_02.gif)