---
title: js学习笔记-cookie的设置
date: 2017-03-16 15:50:57
tags:
---

### 前言：工作中遇到需要在页面初始化的时候生成一个userId，然后存储在本地，当每次发出请求的时候以cookie的形式带上

[相关博客链接>>](http://www.cnblogs.com/shizhouyu/p/3963122.html)

我第一想到的是将生成的使用localstorage去存储userId，然后在请求之前将userId取出来存到cookie中,但是仔细一想，何不直接存到cookie中，然后设置一下过期时间和path，而且localstorage在苹果手机上的支持也不是很好

有了想法，那就撸它。。。

于是我这样写：

```javascript
//当时我要在请求example.com/test/st的时候带cookie
//所以我直接把path赋值为后面的路径，很显然，是我对path了解错了
document.cookie = 'userId=btm' + (Date.now() + Math.random())+';path=/test/st';
```

假设你要请求的网站是www.test.com每次请求的文件路径是不同，而想请求该网站下的不同文件时都能带上同一个cookie，只需将cookie的**path**设置为**'/'**，这样，同域名下不同路径的请求都会带上

同时也得给手动埋的cookie设置一下过期时间，要不然有些浏览器会默认cookie的过期时间为会话结束

所以，正确的写法是这样的：

```javascript
// 设置过期时间
	var exp = new Date();
    exp.setTime(exp.getTime() + 30 * 24 * 60 * 60 * 1000);   //60 * 1000 为一分钟
	document.cookie = 'userId=btm' + (Date.now() + Math.random())+';expires='+exp.toGMTString()+';path=/';   //path='/',只要是域名相同所有的请求都会
```

对cookie方法进行封装：

```javascript
var cookie = {
    set:function(key,val,time){//设置cookie方法
        var date=new Date(); //获取当前时间
        var expiresDays=time;  //将date设置为n天以后的时间
        date.setTime(date.getTime()+expiresDays*24*3600*1000); //格式化为cookie识别的时间
        document.cookie=key + "=" + val +";expires="+date.toGMTString();  //设置cookie
    },
    get:function(key){//获取cookie方法
        /*获取cookie参数*/
        var getCookie = document.cookie.replace(/[ ]/g,"");  //获取cookie，并且将获得的cookie格式化，去掉空格字符
        var arrCookie = getCookie.split(";")  //将获得的cookie以"分号"为标识 将cookie保存到arrCookie的数组中
        var tips;  //声明变量tips
        for(var i=0;i<arrCookie.length;i++){   //使用for循环查找cookie中的tips变量
            var arr=arrCookie[i].split("=");   //将单条cookie用"等号"为标识，将单条cookie保存为arr数组
            if(key==arr[0]){  //匹配变量名称，其中arr[0]是指的cookie名称，如果该条变量为tips则执行判断语句中的赋值操作
                tips=arr[1];   //将cookie的值赋给变量tips
                break;   //终止for循环遍历
            }
        },
      delete:function(key){ //删除cookie方法
         var date = new Date(); //获取当前时间
         date.setTime(date.getTime()-10000); //将date设置为过去的时间
         document.cookie = key + "=v; expires =" +date.toGMTString();//设置cookie
        }
        return tips;
    }
}
```



