---
title: PC端禁止页面缩放
date: 2017-05-26 20:26:46
tags:

---

#### 依赖jQuery的方式：

```javascript
$(document).ready(function () {
      // chrome 浏览器直接加上下面这个样式就行了，但是ff不识别
      $('body').css('zoom', 'reset');
      $(document).keydown(function (event) {

        if ((event.ctrlKey === true || event.metaKey === true)
          && (event.which === 61 || event.which === 107
              || event.which === 173 || event.which === 109
              || event.which === 187  || event.which === 189))
         {
             event.preventDefault();
         }
      });

         $(window).bind('mousewheel DOMMouseScroll', function (event) {
             if (event.ctrlKey === true || event.metaKey) {
                 event.preventDefault();
             }

      });
  });
```

#### 原生方式：

```javascript
document.addEventListener('DOMContentLoaded', function (event) {
            // chrome 浏览器直接加上下面这个样式就行了，但是ff不识别
            document.body.style.zoom = 'reset';
            document.addEventListener('keydown', function (event) {
                if ((event.ctrlKey === true || event.metaKey === true)
                && (event.which === 61 || event.which === 107
                    || event.which === 173 || event.which === 109
                    || event.which === 187  || event.which === 189))
                    {
                       event.preventDefault();
                    }
            }, false);
            document.addEventListener('mousewheel DOMMouseScroll', function (event) {
                if (event.ctrlKey === true || event.metaKey) {
                    event.preventDefault();
                }
            }, false);
        }, false);
```

