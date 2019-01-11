---
title: webpack至于loader的编写
date: 2018-01-31 15:33:06
tags:

---

## `写在前面`

之前看过一篇不错的文章，很适合入门，[传送门](https://segmentfault.com/a/1190000012718374)

本篇文章是基于我看了一些文章后动手写的一些结合平时业务的demo

[完整代码在master分支上](https://github.com/youstde/webpackDemo)

####业务场景：

我现在想写一个插件，这个插件在编写完成后打包只生成一个js，别人用这个插件的时候只需要将这个js引入即可，相应的布局和样式会自动渲染到页面上（简单的一个插件实现）

demo目录结构如下：

![](http://ww1.sinaimg.cn/large/005QDhBjly1fnzuwox5frj30gc0em75n.jpg)

从根目录开始看起，`index.html`是整个项目的容器模板（测试用），`index.js`是入口文件，同时也是插件的核心处理文件，其他就不用多说。然后`template`下面，是插件的核心模板和样式，`loader`目录下面放的就是自己写的`loader`了

下面是webpack的配置：

```javascript
var path = require('path'),
    webpack = require('webpack'),
    ROOT_PATH = path.resolve(__dirname), //当前项目的跟路径（绝对）
    BUILD_DIR = path.resolve(ROOT_PATH, 'build');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: {
    'index': path.resolve(ROOT_PATH, './index.js')
  },
  output: {
    path: BUILD_DIR,
    publicPath: './',
    filename: '[name]/index.js'
  },
    module: {
        rules: [{
            test: /\.html$/,
            use: ['html-loader']
        },{
            test: /\.html$/,
            use: [{
                loader: path.resolve('loader/test-html-loader.js')
            }],
            include: [path.resolve('template/')]
        }]
    },
    plugins: [
      //引入这个插件是为了演示用，编写这个项目的时候不需要生成HTML文件，只需要生成一个JS即可
        new HtmlWebpackPlugin({
          title: '',
          template: path.resolve('index.html'),
          filename: 'index.html',
          //chunks这个参数告诉插件要引用entry里面的哪几个入口
          chunks: ['index'],
          //要把script插入标签里
          inject: 'body'
        })
    ]
}
```

其中核心的部分是：

```json
{
            test: /\.html$/,
            use: [{
                loader: path.resolve('loader/test-html-loader.js')
            }],
            include: [path.resolve('template/')]
        }
```

这个是我自己写的`loader`，处理的是以`html`结尾同时是在`template`目录下的文件,然后就是`test-html-loader`的具体代码：

```Java
var loaderUtils = require('loader-utils');
var fs = require('fs');
module.exports = function (source) {
	var basePath = this.context;
	var cssPath = /(\_include\()+('|")+(.+)+('|")+\)/.exec(source)[3];
	var content = fs.readFileSync(basePath +'/'+ cssPath);
	var all = source.replace(/(\_include\()+('|")+(.+)+('|")+\)/, '<style>'+content+'</style>');
  return all;
}
```

解释一下，这个loader主要做的就是匹配`_include(PATH)`,然后拿到文件路径,读取对应文件之后替换掉`_include(PATH)`,最后返回处理后的文件，为什么要这么做，是因为我的center.html里是这样写的：

```html
<div>
_include("center.css")
	<h2>test</h2>
	<h3>xixi</h3>
	<button>click</button>
</div>
```

目的就是将css打到html中，然后index.js再去以模板的形式引入：

```javascript
var tpl = require('./template/center.html');
var parentEl = document.createElement('div');
parentEl.innerHTML = tpl;
parentEl.querySelector('button').onclick = function() {
	console.log(111);
}
document.body.appendChild(parentEl);
```

这样，当我打包完成以后，打开index.html就可以看到

![](http://ww1.sinaimg.cn/large/005QDhBjly1fnzvmsu4cnj314o0kcjsc.jpg)

