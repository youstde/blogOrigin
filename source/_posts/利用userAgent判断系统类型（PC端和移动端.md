---
title: 利用userAgent判断系统类型（PC端和移动端
date: 2017-03-28 10:51:51
tags:
---

```javascript
require(['jquery'], function($) {

    var ua = navigator.userAgent;
    console.log(ua);

    // 判断pc系统类型
    if (ua.indexOf("Windows NT 5.1") !== -1) console.log("Windows Vista");
    if (ua.indexOf("Windows NT 6.1") !== -1) console.log("Windows 7");
    if (ua.indexOf("Windows NT 6.2") !== -1) console.log("Windows 8");
    if (ua.indexOf("Windows NT 10") !== -1) console.log("Windows 10");
    if (ua.indexOf("Mac OS X 10_7") !== -1) console.log("OSX 10.7");
    if (ua.indexOf("Mac OS X 10.8") !== -1) console.log("OSX 10.8");
    if (ua.indexOf("Mac OS X 10_8") !== -1) console.log("OSX 10.8");
    if (ua.indexOf("Linux") !== -1) console.log("Linux");

    // 判断手机系统类型
    if (ua.indexOf("Android") !== -1) console.log("Android");
    if (ua.indexOf("Android 2.3") !== -1) console.log("Android 2.3");
    if (ua.indexOf("Android 4.0") !== -1) console.log("Android 4.0");
    if (ua.indexOf("Android 4.1") !== -1) console.log("Android 4.1");
    if (ua.indexOf("Windows Phone 8") !== -1) console.log("Windows Phone 8");
    if (ua.match(/OS 7_[0-9_]+ like Mac OS X/i)) console.log("iOS7");
    if (ua.match(/OS 6_[0-9_]+ like Mac OS X/i)) console.log("iOS6");
    if (ua.match(/OS 5_[0-9_]+ like Mac OS X/i)) console.log("iOS5");
    if (ua.match(/OS 4_[0-9_]+ like Mac OS X/i)) console.log("iOS4");

    // 判断浏览器类型
    if (ua.indexOf("MSIE 9") !== -1) console.log("Internet Explorer 9");
    if (ua.indexOf("MSIE 8") !== -1) console.log("Internet Explorer 8");
    if (ua.indexOf("MSIE 7") !== -1) console.log("Internet Explorer 7");
    if (ua.indexOf("MSIE 6") !== -1) console.log("Internet Explorer 6");
    if (ua.indexOf("Firefox") !== -1) console.log("Firefox");
    if (ua.indexOf("Chrome") !== -1) console.log("Chrome");
    if (ua.indexOf("Mobile Safari") !== -1 && ua.indexOf("Android") !== -1) console.log("Mobile Safari");
    if (ua.indexOf("Safari") !== -1) console.log("Safari");

})

```

![](http://ww1.sinaimg.cn/large/005QDhBjgy1fe2dm76il0j31uq0hmtan.jpg)

