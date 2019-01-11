---
title: localStorage还能这么用
date: 2017-10-10 19:46:12
tags:
---

# [localStorage 还能这么用(转载)](https://iammapping.com/the-other-ways-to-use-localstorage/)

![img](http://self-storage.b0.upaiyun.com/2017/10/09/150756387698667375.png)

HTML5中 Web Storage 的出现，主要是为了弥补使用 Cookie 作为本地存储的不足。Cookie 存储的数据量非常小，而且数据会自动携带到请求头里，但服务器端可能并不关心这些数据，所以会造成带宽的浪费。

Web Storage 提供了两个存储对象：localStorage 和 sessionStorage。

sessionStorage 存储的数据仅在本次会话有用，会话结束后会自动失效，而且数据仅在当前窗口有效，同一源下新窗口也访问不到其他窗口基于 
sessionStorage 存储的数据。也是由于这些特性，导致 sessionStorage 的使用场景会比较少。

localStorage 可以永久存储，而且同源下数据多窗口也能共享，。看起来很美好，但 localStorage 也有短板，绝大多数浏览器有 5M 的大小限制。但是这不足以成为大家使用 localStorage 的障碍，要知道 Cookie 只有 4K 的大小，多了一千多倍，偷着乐吧。

## localStorage 的基本使用

讲这个内容的文章网上太多了，现在再来讲有点老套了。这里就不做更多介绍了，不清楚的读者可以看[这里](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)。

有两点需要注意一下。在 `setItem` 时，可能会达到大小限制，最好加上错误捕捉 [1](https://iammapping.com/the-other-ways-to-use-localstorage/#ref-error)：

```javascript
try {  
  localStorage.setItem(key, value);
} catch(e) {
  if (isQuotaExceeded(e)) {
    // Storage full, maybe notify user or do some clean-up
  }
}

function isQuotaExceeded(e) {  
  var quotaExceeded = false;
  if (e) {
    if (e.code) {
      switch (e.code) {
        case 22:
          quotaExceeded = true;
          break;
        case 1014:
          // Firefox
          if (e.name === 'NS_ERROR_DOM_QUOTA_REACHED') {
            quotaExceeded = true;
          }
          break;
      }
    } else if (e.number === -2147024882) {
      // Internet Explorer 8
      quotaExceeded = true;
    }
  }
  return quotaExceeded;
}
```

另外在存储容量快满时，会造成 `getItem` 性能急剧下降 [2](https://iammapping.com/the-other-ways-to-use-localstorage/#ref-performance)。

我们下面看看 localStorage 有哪些脑洞大开的用法。

## 缓存静态文件

你不禁要问，HTTP 协议不是本来就支持缓存文件吗（之前写过一篇文章[《Web缓存之HTTP指南》](https://iammapping.com/the-http-guide-of-web-cache/)），为什么还要使用 localStorage 来缓存？为了可编程化，通俗一点说就是把命运握在自己手中。

HTTP 协议的缓存，可以由用户浏览器清除或禁用缓存，也可以由 Web 服务器设置过期时间或不缓存。对于前端工程师，这更像是一个黑盒，想要决定文件是访问缓存还是访问远程显得有些力不从心了。

使用 localStorage 控制文件缓存的方式有两种：

1. 使用 Loader 加载静态文件
2. 借助服务器端将静态文件 inline 化

这两种方式一般都会提前做好缓存过期策略，通常是使用版本号来控制，下面还会细讲。否则文件新版上线，用户客户端还是旧版，这就麻烦大了，而且这类问题，还不好调试不好重现。

### 使用 Loader 加载静态文件

由于请求都是动态发出的，所以可以对请求拦截处理。大致流程如下：

1. 查看请求的文件 url 是否有缓存到 localStorage

    

   ​

   1. 如果没有，到第 2 大步
   2. 如果有，判断文件是否过期或版本号是否匹配
      1. 过期或不匹配，到第 2 大步
      2. 文件内容有效，到第 4 大步

2. 请求远程文件

3. 缓存最新文件内容

4. 执行文件内容

这个方式有个开源库：[basket.js](https://github.com/addyosmani/basket.js)。

### 借助服务器端将静态文件 inline 化

这个方式比上面那种更进一步，在第一次响应时把需要放入 localStorage 的文件都内联进 html 中，后面每次响应只要文件版本没有变化，都是渲染一段从 localStorage 加载该文件的代码。这样做的好处是可以有效减少请求次数，即使是第一次。

版本号不匹配（版本号可记在 Cookie 中，第一次访问没有版本号），服务端响应内容：

```javascript
<script>  
function script2ls(id) {  
  var script = document.getElementById(id);
  if (script) {
    localStorage[id] = script.innerHTML;
  }
}
</script>

<script id="jquery.js">...jquery source code...</script>  
<script>script2ls('jquery.js')</script>  
```

版本号匹配，服务端响应内容：

```javascript
<script>  
function ls2script(id) {  
  var script = document.createElement('script');
  script.text = localStorage[id];
  document.head.appendChild(script);
}
</script>

<script>ls2script('jquery.js')</script>  
```

不过使用 localStorage 缓存文件会有 XSS 的风险，而且造成的伤害可能是永久的 [3](https://iammapping.com/the-other-ways-to-use-localstorage/#ref-xss)。

## 同源窗口通信

你可能不禁又要问，不是有 postMessage 吗？没错 postMessage 确实可以用于窗口或 iframe 间通信，但是前提是你必须拿到打开新窗或 iframe 的句柄对象：

```javascript
var popup = window.open(...popup details...);  
popup.postMessage("hello there!", "http://example.com");  
```

这样在新窗中再打开新窗，似乎就不好传递消息了。

你可能还想问，为什么要在窗口间通信？好问题，没有应用场景的技术都是耍流氓。像多窗口共用的一些组件，而且对数据实时同步都有较高要求的都会是这个技术的应用场景。比如通知中心上面的未读数量，两个窗口，A 窗口更新为 8，切到 B 窗口还是 9，这就造成了体验不一致，这个例子可能还觉得无关痛痒；再比如购物车，两个产品窗口，A 窗口添加到购物车，切到 B 窗口添加到购物车，发现没有 A 添加的产品，这样就比较严重了。这当然也可以通过每个窗口都与后台建立连接来更新，但用户如果开十几个窗口就开销大了。

有了同源窗口通信，我们就可以只有一个窗口与后台建立连接，收到更新后，广播给其他窗口就可以。说了这么多，实现原理是怎样的呢？

![storage event](http://self-storage.b0.upaiyun.com/2017/10/09/150756219537917384.png)

其实原理也简单，每次 localStorage 中有任何变动都会触发一个 `storage` 事件，所有窗口都监听这个事件，一旦有窗口更新 localStorage，其他窗口都会收到通知，根据事件中的 `key` 把不关心的变动过滤掉。原理是很简单，但是要实现一套完整的广播机制还是有些复杂，你需要：

- 管理好每个窗口的唯一 ID
- 防止消息重复
- 防止消息发给不关心的窗口
- 窗口心跳 keep alive
- 主窗口选举
- ...

不用担心，已经有了不错的开源实现：[diy/intercom.js](https://github.com/diy/intercom.js)、[tejacques/crosstab](https://github.com/tejacques/crosstab)

## 其他

### 作为前端 DB 的存储介质

你可能不满足于用键值对保存数据，你还想保存更复杂的数据结构。

灵活存取 json 格式的数据：[typicode/lowdb](https://github.com/typicode/lowdb#install)

通过 sql 对数据 CURD 操作：[agershun/alasql](https://github.com/agershun/alasql#localstorage-and-dom-storage)

### 表单自动持久化

在填写表单时，遇到浏览器奔溃或者误操作导致填写内容丢失，此刻用户的内心也应该是奔溃的。误操作还可以加一个 `beforeunload` 事件，在关闭浏览器或跳出当前页前提醒一下用户。那浏览器崩溃呢，将数据变更实时保存到后台，这样似乎开销很大，实时保存到 localStorage 是个不错的解决方案，真巧，也有一个开源实现：[simsalabim/sisyphus](https://github.com/simsalabim/sisyphus)

## 总结

文件缓存现在有更好的解决方案，没错，说的就是 Service Worker 的 [CacheStorage](https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage)。

不过多了解一些脑洞大开的想法，有利于拓展解决问题的思路。

Enjoy open source ❤️

参考： 
1：[Always catch LocalStorage security and quota exceeded errors](http://crocodillon.com/blog/always-catch-localstorage-security-and-quota-exceeded-errors) 
2：[Measuring localStorage Performance](http://www.stevesouders.com/blog/2014/02/11/measuring-localstorage-performance/) 
3：[使用 SRI 增强 localStorage 代码安全](https://imququ.com/post/enhance-security-for-ls-code.html)