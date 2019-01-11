---
title: atom中实现实时刷新及移动端预览
date: 2017-01-03 17:16:06
tags:
---

### Browsersync

#### 安装

```
sudo npm install -g browser-sync
```

#### 使用

1.用atom打开一个项目

2.使用cd切换到对应的项目的文件夹下

3.在终端输入命令

```
browser-sync start --server --browser "Google Chrome" --files "stylesheets/*.css, *.html"
```

也可以使用默认的浏览器进行访问

```
browser-sync start --server --files "stylesheets/*.css, *.html"
```

![Aaron Swartz](http://ww3.sinaimg.cn/large/005QDhBjgw1fbdkp3du4dj30fm05x75h.jpg)

4.每次改动代码并保存后，浏览器都会自动进行更新

5.保证电脑和手机在同一个局域网下面，在移动端浏览器中输入External中的网址，这样就能在移动端实时显示页面效果了