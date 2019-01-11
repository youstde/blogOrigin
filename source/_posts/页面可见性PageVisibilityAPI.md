---
title: 页面可见性PageVisibilityAPI
date: 2017-02-03 14:59:47
tags:
---

### HTML5“新的”API：Page Visibility

[点击到源博客](http://www.alloyteam.com/2012/11/page-visibility-api/)

#### 1.用途

通过visibilityState的值检测页面当前是否可见，以及打开网页的时间等；

```javascript
//当页面是打开的时候visibilitySatte的值是：visible
//当tab切面被切换或者浏览器窗口被最小化的时候，visibitySatte的值是hidden
```

在页面被切换到其他后台进程的时候，自动暂停音乐或视频的播放

### 2.使用

支持Page VIsibility的浏览器在document上会添加hidden和visibilityState两个属性。

```javascript
function getHiddenProp() {
          return 'hidden' in document ? 'hidden' : function() {
            var r = null;
            ['webkit', 'moz', 'ms', 'o'].forEach(function(prefix) {
              if((prefix + 'Hidden') in document) {
                return r = prefix + 'Hidden';
              }
            });
            return r;
          }();
      }
      var l = getHiddenProp(),
          k = l.slice(0, -6)
      console.log(l)
      document.addEventListener(k+'visibilitychange', function onVisibilityChange(e) {
	    //do some thing...
      console.log(document[l]) //获取当前页面hidden的值：true/false
      console.log(document.visibilityState) //获取当前页面visibilityState的值
    });
```

