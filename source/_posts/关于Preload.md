---
title: 资源预加载
date: 2017-11-24 10:08:32
Tags:
---

[原文请走这里](http://www.jianshu.com/p/24ffa6d45087)

前言：关于资源预加载，其实民间有很多种方式，有自己实现的，也有浏览器自带的，今天我只说一下浏览器自带 的几种方式

`prefetch` `subresource` `Preload`

其中prefetch和subresource是之前就已经出现的，Preload是个新鲜的东西，也是最近刷博客看到的，觉得不错就转载一下

####区别

事实上，关于预加载，我们已经有了`<link rel=“prefetch”>`,而且浏览器兼容性表现也不错![](http://ww1.sinaimg.cn/large/005QDhBjgy1flsyjwmbu6j30h508kjrr.jpg)

除此之外，Chrome还支持过`<link rel=“subresource”>`。

而Preload的存在的意义在于，他跟现在已经壮烈牺牲的subresource很像，同时又没有subresource的缺点。

## `<link rel=“prefetch”>`

是加载下一个页面可能用到的资源，并不是针对于当前页。因此该方法的加载优先级非常低自然，相比当前页面所需的资源，未来可能会用到的资源就没那么重要了），也就是说该方式的作用是加速下一个页面的加载速度。

## `<link rel=“subresource”>`

的设计初衷是处理当前页面，但最后还是壮烈牺牲了。因为开发者无法控制资源的加载优先级，因此浏览器（其实也只有 Chrome 和基于 Chrome 的浏览器）在处理此类标签时，优先级很低，到底有多低呢？这么说吧，在大多数情况下，用了等于没用。

## `<link rel=“preload”>`

Preload是为处理当前页面所生，这点和 subresource 一样，但他们之间有着细微且意义重大的区别。Preload 有 as 属性，这让浏览器可做一些 subresource 和 prefetch 无法实现的事：

* 浏览器可以设置正确的资源加载优先级，这种方式可以确保资源根据其重要性依次加载， 所以，Preload既不会影响重要资源的加载，又不会让次要资源影响自身的加载。
* 浏览器可以确保请求是符合内容安全策略的，比如，如果我们的安全策略是Content-Security-Policy: script-src 'self'，只允许浏览器执行自家服务器的脚本，as 值为 script 的外部服务器资源就不会被加载。
* 浏览器能根据 as 的值发送适当的 Accept 头部信息
* 浏览器通过 as 值能得知资源类型，因此当获取的资源相同时，浏览器能够判断前面获取的资源是否能重用。

Preload 的与众不同还体现在 onload 事件上（至少在 Chrome 中，prefetch 和 subresource 是不支持的）。也就是说你可以定义资源加载完毕后的回调函数。

```html
<link rel="preload" href="..." as="..." onload="preloadFinished()">
```

例:

```html
<link rel="preload" href="late_discovered_thing.js" as="script">
```

as 属性的作用是告诉浏览器被加载的是什么资源，可能的 as 值包括：

- "script"
- "style"
- "image"
- "media"
- "document"

忽略 as 属性，或者错误的 as 属性会使 preload 等同于 XHR 请求，浏览器不知道加载的是什么，因此会赋予此类资源非常低的加载优先级。

## 响应式加载

preload 是一个link，根据规范有一个media 属性（现在 Chrome 还不支持，不过快了），该属性使得选择性加载成为可能。

有什么用处呢？假设你的站点同时支持桌面和移动端的访问，在使用桌面浏览器访问时，你希望呈现一张可交互的大地图，而在移动端，一张较小的静态地图就足够了。

你肯定不想同时加载两个资源，现在常见的做法是通过 JS 判断当前浏览器类型动态地加载资源，但这样一来，浏览器的预加载器就无法及时发现他们，可能耽误加载时机，影响用户体验和 SpeedIndex 评分。

怎样才能让浏览器尽可能早的发现这些资源呢？还是 Preload!

通过 Preload，我们可以提前加载资源，利用 media 属性，浏览器只会加载需要的资源。

```
<link rel="preload" as="image" href="map.png" media="(max-width: 600px)">
<link rel="preload" as="script" href="map.js" media="(min-width: 601px)">
```

## HTTP 头

Preload 还有一个特性是其可以通过 HTTP 头信息被呈现。也就是说上文中大多数的基于标记语言的声明可以通过 HTTP 响应头实现。（唯一的例外是有 onload 事件的例子，我们不可能在 HTTP 头信息中定义事件处理函数。）

```
Link: <thing_to_load.js>;rel="preload";as="script"
Link: <thing_to_load.woff2>;rel="preload";as="font";crossorigin
```

这一方式在有些场景尤其有用，比如，当负责优化的人员与页面开发人员不是同一人时（也就是说优化人员可能无法或者不想修改页面代码），还有一个杰出的例子是外部优化引擎（External optimization engine），该引擎对内容进行扫描并优化。

## 特征检查 （Feature Detection）

前面所有的列子都基于一种假设——浏览器一定程度上支持 preload，至少实现了脚本和样式加载等基本功能。但如果这个假设不成立了。一切都将是然并卵。

为了判断浏览器是否支持 preload，我们修改了 DOM 的规范从而能够获知 rel 支持那些值（是否支持 rel=‘preload’）。

至于如何进行检查，原文中没有，但 Github有一段代码可供参考。

```javascript
var DOMTokenListSupports = function(tokenList, token) {
  if (!tokenList || !tokenList.supports) {
    return;
  }
  try {
    return tokenList.supports(token);
  } catch (e) {
    if (e instanceof TypeError) {
      console.log("The DOMTokenList doesn't have a supported tokens list");
    } else {
      console.error("That shouldn't have happened");
    }
  }
};

var linkSupportsPreload = DOMTokenListSupports(document.createElement("link").relList, "preload");
if (!linkSupportsPreload) {
  // Dynamically load the things that relied on preload.
}
```