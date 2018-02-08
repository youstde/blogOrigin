---
title: sass的使用详解
date: 2016-12-06 14:28:08
tags:
---

### 1.安装sass

在这之前，你得已经安装了[ruby](http://rubyinstaller.org/downloads)，如果官网打不开，那就[点击这里](http://www.cnblogs.com/daguo/p/4097263.html)

ruby安装完成之后，依次执行以下代码：

（1）首先，将默认的镜像源移除：

```
gem sources --remove https://rubygems.org/
```

（2）然后添加淘宝的镜像源：

```
gem sources -a https://ruby.taobao.org/
```

（2）如果你系统不支持https，请将淘宝源更换成：

```
gem sources -a http://gems.ruby-china.org
```

（3）查看镜像源：

​	此时请确保只有ruby.taobap.org

```
gem sources -l
//出现下面的效果是镜像切换成功了
*** CURRENT SOURCES ***
https://ruby.taobao.org
```

（4）安装sass：

```
gem install sass
//如果安装时候提示你没有添加任何什么什么到任何仓库中，直接在上面代码前加上
sudo
```

### 2.使用Sass

我选择的是使用Scss去编写，因为他更符合我们的日常代码风格和规范，看着也爽

监视scss文件的变化，然后编译生成css文件

```
sass --watch sass:css --style STYLE
//STYLE分为四种：
    　　* nested：嵌套缩进的css代码，它是默认值。

    　　* expanded：没有缩进的、扩展的css代码。

    　　* compact：简洁格式的css代码。

    　　* compressed：压缩后的css代码。
```

1.变量的申明：

```scss
$red: #ed2a2a;
```

2.重复头部的提取：

传统写法：

```css
ul li{
  font-size: 16px;
  font-family: sans-serif;
  font-weight: bold;
}
```

改进写法：

```scss
ul {
  li {
    font: {
      size: 16px;
      family: sans-serif;
  	  weight: bold;
    }
  }
}
```

3.重复样式的提取：

使用@mixin 去申明

在需要的地方使用@include去引用

```scss
/*申明*/
@mixin st( $color, $border-color ) {
  border-bottom: 1px solid $border-color;
  color: $color;
  a {
    color: $color;
  }
}
/*调用*/
.show {
  @include st( #366cb6, #b85318 );
}
```

4.继承

```scss
.st {
  color: pink;
  border-radius: 10px;
}
.sts {
  width: 100px;
  height: 100px;
  @extend .st;
}
```

5.Partials

css的模块化

将某一块的css抽离出来，写成一个新的scss的文件，但是得以下划线开头

然后在主scss中通过@import中引入

引入的时候不需要加下划线和后缀

![Aaron Swartz](http://ww2.sinaimg.cn/large/005QDhBjgw1fah3iewjp4j31kw0vi7dd.jpg)

持续更新中。。。

