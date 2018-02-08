---
title: 微信小程序之template
date: 2016-11-24 15:19:24
tags:
---

#### 此文主要是介绍小程序中自定义的template的引用和编写

### 1.template的编写

在page同级新建一个文件夹存放自定义的template

然后分别新建一个wxml和wxss文件

coding….

## component.wxml

```html
<!--S=主模板-->
<template name = 'wxParse'>
    <view class = 'st'>this is template wxParse</view>
    <block wx:for-items = '{{temData}}'>
        <template is = 'parse1' data = '{{item}}'/>
    </block>
</template>
<!--E=主模板-->

<!--S=子模板-->
<template name = 'parse1'>
        <text>{{item}}</text>
</template>
<!--E=子模板-->
```

## component.wxss

```css
.st {
    width: 100%;
    height: 80rpx;
    background-color: pink;
    line-height: 80rpx;
    padding: 0 20rpx;
    box-sizing: border-box;
}
```

### 2.模板的引用

首先在主文件中写入如下代码

```html
<import src = '../../components/component.wxml'/>
```

然后就是模板的使用

```html
<view class = 'main'>
    <template is = 'wxParse' data = '{{temData}}'/>
</view>
```

其中主文件的js中的data数据如下：

```javascript
Page({
  data: {
    name: 'jack',
    age: '30'
  }
})
```

当然我们也可以这样写

通过判断`item`是否能被`2`整除，进而选择不同的组件进行渲染

```html
<template name="odd">
  <view> odd </view>
</template>
<template name="even">
  <view> even </view>
</template>

<block wx:for="{{[1, 2, 3, 4, 5]}}">
    <template is="{{item % 2 == 0 ? 'even' : 'odd'}}"/>
</block>
```

### 3.模板样式的复用

把写在components文件夹中的component.wxss引入到App.wxss中，通过以下代码引入：

```css
@import './components/component.wxss'
```

### 4.最后结果为：

![Aaron Swartz](http://ww1.sinaimg.cn/large/005QDhBjgw1fa38omk34jj31520fy40i.jpg)