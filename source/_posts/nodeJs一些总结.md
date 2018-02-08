---
title: nodeJs一些总结
date: 2017-02-04 11:10:47
tags:
---

## nodeJS

### 1.[superagent](http://visionmedia.github.io/superagent/ )

是个http方面的库，用以发送get和post请求

### 2.[cheerio](https://github.com/cheeriojs/cheerio )

可以理解为是一个NodeJs版的jQuery，用来从网页中以 css selector 取数据，使用方式跟 jquery 一样一样的。

### 3.node取参的方式

1.req.body

2.req.query

3.req.params

4.req.param()

其中，req.body和req.query的不同

req.body：

* 解析body不是nodeJS默认提供的，所以需要载入body-parser中间件才可以使用req.body
* 此方法通常用来解析POST请求中的数据

req.query:

* 有nodejs默认提供，无需载入中间件