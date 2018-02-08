---
title: setTimeout的工作机制
date: 2017-01-03 17:31:26
tags:
---

[点我到原文](https://mp.weixin.qq.com/s?__biz=MzI1MTE2NTE1Ng==&mid=2649515867&idx=1&sn=971a3e41da08ddf2da200d9d07af0fb0&chksm=f1efe7d0c6986ec688a746ece15f52c8df78bca37ca2609e75199f5c3fbbabd3fbcc00179885&scene=0&key=564c3e9811aee0abcc036cb111e6e7bdbe3938a8756b5bf3b98a1696b2f16c1e6e3a1b4af159d1ae1dd3e71ee5fae4e0b6655bd9f37cc81efb1174bf3ef39b43f874bc6a0482348422cc5245dfae917f&ascene=0&uin=MzIxNTY1NTU%3D&devicetype=iMac+MacBookPro11%2C1+OSX+OSX+10.12.1+build(16B2555)&version=12010210&nettype=WIFI&fontScale=100&pass_ticket=g24dIjS%2F70EF4QPCYwRMInMa218z6XagvevxLr5Mbzc%3D)，[另外一篇写的很好的文章](https://juejin.im/post/59c25c936fb9a00a3f24e114?utm_source=gold_browser_extension),[还有一个神奇的网站](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwpzZXRUaW1lb3V0KGZ1bmN0aW9uIHRpbWVvdXQoKSB7CiAgICBjb25zb2xlLmxvZygiQ2xpY2sgdGhlIGJ1dHRvbiEiKTsKfSwgMCk7CmNvbnNvbGUubG9nKCJIaSEiKTsKCgoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

如果想细看的话，请点击上面的链接，我只做一点总结和笔记

定时器其实就相当于一个异步操作，而异步的代码必须等到主线程上的同步代码执行完成之后才会去被执行

![Aaron Swartz](http://ww4.sinaimg.cn/large/005QDhBjgw1fbdl9lh1xdj30z60r8jtk.jpg)

![Aaron Swartz](http://ww4.sinaimg.cn/large/005QDhBjgw1fbdlc6pj1gj31100e6gmf.jpg)

![Aaron Swartz](http://ww3.sinaimg.cn/large/005QDhBjgw1fbdldjrnfuj30z80gwab5.jpg)

![Aaron Swartz](http://ww4.sinaimg.cn/large/005QDhBjgw1fbdlfutkzwj30zu0f03zq.jpg)

