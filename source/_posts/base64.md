---
title: 用户上传图片压缩问题
date: 2016-11-22 14:31:16
tags:
---

### 使用base64进行压缩，同时使用cavas进行图片的压缩上传

## 使用cavas生成base64

```javascript
//生成图片的base64编码
function convertImgToBase64(url, callback, outputFormat){
     //html5 的convas画布
     var canvas = document.createElement('CANVAS');
     var ctx = canvas.getContext('2d');
     var img = new Image;
     img.crossOrigin = 'Anonymous';
     img.onload = function(){
       var width = img.width;
       var height = img.height;
       // 按比例压缩4倍
       //var rate = (width<height ? width/height : height/width)/4;
       //原比例生成画布图片
       var rate = 1;
       canvas.width = width*rate;
       canvas.height = height*rate;
       ctx.drawImage(img,0,0,width,height,0,0,width*rate,height*rate);
 // canvas.toDataURL 返回的是一串Base64编码的URL，当然,浏览器自己肯定支持
        var dataURL = canvas.toDataURL(outputFormat || 'image/png');
        callback.call(this, dataURL);
        canvas = null;
      };
      img.src = url;
    }

```

## 创建一个代表图片的URL

```javascript
//createobjecturl()静态方法创建一个包含了DOMString代表参数对象的URL。该url的声明周期是在该窗口中.也就是说创建浏览器创建了一个代表该图片的Url.
function getObjectURL(file) {
     var url = null ;
      if (window.createObjectURL!=undefined){
     // basic
        url = window.createObjectURL(file);
      } else if (window.URL!=undefined){
     // mozilla(firefox)
        url = window.URL.createObjectURL(file) ;
      } else if (window.webkitURL!=undefined){
     //web_kit or chrome
        url = window.webkitURL.createObjectURL(file);
      }
      return url ;
     }
 });
```

