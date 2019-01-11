---
title: flex.js的使用
date: 2016-11-23 16:17:40
tags:
---

 ### 安装flex.csss

通过npm安装：

```
npm install --save flex.css
```

### 1.设置主轴方向

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>设置主轴方向</title>
    <link rel="stylesheet" href="./flex.css">
    <style type="text/css">
        .box {
            width: 150px;
            height: 150px;
            border: 1px solid #ddd;
        }
        .item {
            width: 30px;
            height: 30px;
            line-height: 30px;
            color: #fff;
            text-align: center;
        }
    </style>
</head>

<body>
    <h2>从上到下</h2>
    <div class="box" flex="dir:top">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>从右到左</h2>
    <div class="box" flex="dir:right">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>从下到上</h2>
    <div class="box" flex="dir:bottom">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>从左到右（默认）</h2>
    <div class="box" flex="dir:left">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
</body>

</html>
```



![Aaron Swartz](http://ww1.sinaimg.cn/large/005QDhBjgw1fa24yf2tz7j30io0i20t1.jpg)

![Aaron Swartz](http://ww3.sinaimg.cn/large/005QDhBjgw1fa2507iawgj30kc0iugm1.jpg)

### 主轴对齐方式

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>主轴对齐方式</title>
    <link rel="stylesheet" href="./flex.css">
    <style type="text/css">
        .box {
            width: 150px;
            height: 150px;
            border: 1px solid #ddd;
        }
        .item {
            width: 30px;
            height: 30px;
            line-height: 30px;
            color: #fff;
            text-align: center;
        }
    </style>
</head>

<body>
    <h2>从右到左</h2>
    <div class="box" flex="main:right">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>从左到右（默认）</h2>
    <div class="box" flex="main:left">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>两端对齐</h2>
    <div class="box" flex="main:justify">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>居中对齐</h2>
    <div class="box" flex="main:center">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
</body>

</html>
```

![Aaron Swartz](http://ww1.sinaimg.cn/large/005QDhBjgw1fa2559bwsnj311a0kk3z8.jpg)

![Aaron Swartz](http://ww4.sinaimg.cn/large/005QDhBjgw1fa2567w7thj312i0iu0t9.jpg)

### 交叉轴对齐方式

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>交叉轴对齐方式</title>
    <link rel="stylesheet" href="./flex.css">
    <style type="text/css">
        .box {
            width: 150px;
            height: 150px;
            border: 1px solid #ddd;
        }
        .item {
            width: 30px;
            /*height: 30px;*/
            line-height: 30px;
            color: #fff;
            text-align: center;
        }
    </style>
</head>

<body>
    <h2>从上到下（默认）</h2>
    <div class="box" flex="cross:top">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>从下到上</h2>
    <div class="box" flex="cross:bottom">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>基线对齐</h2>
    <div class="box" flex="cross:baseline">
        <div class="item" style="font-size: 30px; background: red;">1</div>
        <div class="item" style="font-size: 12px; background: blue;">2</div>
        <div class="item" style="font-size: 40px; background: #000;">3</div>
    </div>
    <h2>居中对齐</h2>
    <div class="box" flex="cross:center">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
    <h2>高度并排铺满</h2>
    <div class="box" flex="cross:stretch">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
</body>

</html>
```

![Aaron Swartz](http://ww1.sinaimg.cn/large/005QDhBjgw1fa259lwgdnj310k0nqdgm.jpg)

![Aaron Swartz](http://ww4.sinaimg.cn/large/005QDhBjgw1fa25a8oog9j312m0eyt95.jpg)

### 水平居中

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>水平居中</title>
    <link rel="stylesheet" href="./flex.css">
    <style type="text/css">
        .box {
            width: 150px;
            height: 150px;
            border: 1px solid #ddd;
        }
        .item {
            width: 30px;
            height: 30px;
            line-height: 30px;
            color: #fff;
            text-align: center;
        }
    </style>
</head>

<body>
    <h2>水平居中</h2>
    <div class="box" flex="main:center cross:center">
        <div class="item" style="background: red;">1</div>
        <div class="item" style="background: blue;">2</div>
        <div class="item" style="background: #000;">3</div>
    </div>
</body>

</html>
```

![Aaron Swartz](http://ww1.sinaimg.cn/large/005QDhBjgw1fa25ce6e8bj31980najs4.jpg)

