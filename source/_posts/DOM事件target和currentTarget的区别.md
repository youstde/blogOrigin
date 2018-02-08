---
title: DOM事件target和currentTarget的区别
date: 2017-10-23 15:12:25
tags:
---

#### 事件，就是文档或浏览器窗口发生的一些特定交互的瞬间。

这次先来说说事件对象。在触发DOM上的某个事件时，会产生一个事件对象event,这个对象中包含着所有与事件有关的信息。
所有的浏览器都支持event对象，但是支持的方式不同。那事件对象怎么得到呢:

```javascript
var oBtn = document.querySelector('myBtn');
oBtn.onclick = function(event){
    console.log(event);
}
oBtn.addEventListener("click",function(event){
    console.log(event);
},false)
```

很简单是吧，在触发特定事件的时候，这个变量event保存的就是event对象

```javascript
<input type="button" onclick="alert(event)"/>
```

这样使用也是一样的，可以获取到event对象。
不同的触发事件会有不同的属性和方法,当然，所有的事件都会有一些相同的属性和方法，这里就不一一列举了，有兴趣的同学可以看看高级程序设计这本书的事件这一章。

这里列举几个常用的，currentTarget与target,这两个属性看上去挺相似的，书上对这两个属性的解释是
currentTarget（其事件处理程序当前正在处理事件的那个元素），target(事件的目标)。通过自己写的demo,我对这两个属性的理解是，currentTarget(你绑定事件的那个元素), target(触发事件的那个目标)

```javascript
 <div class="box">
      <input type="button" value="点我">
</div>

//js部分
var oBox = document.querySelector('.box');
var oBtn = document.querySelector('input');

oBtn.onclick=function(event){
     console.log(this);    //button
     console.log(event.target);    //button
     console.log(event.currentTarget);    //button
}


oBox.onclick=function(event){
     console.log(this);    //div
     console.log(event.target);    //button
     console.log(event.currentTarget);    //div
}
```

通过demo可以看出 this始终等于currentTarget,也就是注册事件处理程序的元素，target就是触发事件的实际目标。

点击button的时候由于事件处理程序注册于button之上，所以处理事件的目标也是button。
如果只是在div上注册点击事件处理程序，虽然事件是由button出发的，但是它本身并没有注册这个事件处理程序，所以会向上冒泡，找到div元素。

说道事件冒泡，顺带说一下事件代理，

现在有个场景，一个ul下面有100个li标签，现在给每个li标签绑定相同事件，该怎么去做。

有人会说，循环遍历，这个很强，但是很傻，如果是1000个呢，还是遍历吗

所以现在就需要使用事件代理，

方法就是给所有的li标签的父元素（也就是ul）绑定一个事件，然后通过event的target去找到触发事件的那个li