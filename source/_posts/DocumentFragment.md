---
title: DocumentFragment的使用
date: 2016-11-23 15:19:54
tags:
---

### 真实项目中频繁操作dom是很奢侈的，而如果想动态渲染页面，DocumentFragment的一个很好的选择

##### 比如下面这个例子

当我想动态的将`li`标签添加到ul标签中，我不需要每次循环的时候把`li`添加到`ul`中，而是先暂存到`fragObj`中，等循环结束之后一次将所有的li添加到页面中

## html

```html
  <div id="box">
    <ul id = 'meul'>

    </ul>
  </div>
```

## Js

```javascript
//=====================S=DocumentFragment=========================
//==如果是要频繁的操作真实dom，那就先创建一个片段，先存储到片段中，最后再一次性的放到真实dom中
  var box = document.getElementById( 'box' ),
      fragObj = new DocumentFragment(),
      ulObj = document.getElementById( 'meul' );
  for( var i= 0; i< 3; i++ ) {
    var li = document.createElement( 'li' );
    li.innerText = i;
    fragObj.appendChild( li );
    // ulObj.appendChild( li );
  }
  ulObj.appendChild( fragObj );
//=====================E=DocumentFragment=========================
```



