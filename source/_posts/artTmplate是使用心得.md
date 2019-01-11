---
title: artTmplate使用心得
date: 2016-11-25 13:42:07
tags:
---

### 首先下载atrTemplate

```
npm install art-template
```

### 模板的编写

其中模板的`id`为`myTemplate`

```html
<ul>
      {{each list}}
  <!--如果list中的每一项有title，那就显示h3标签-->
        {{if $value.title}}
          <h3>{{$value.title}}</h3>
        {{/if}}
    <!--如果list中的每一项有text，那就显示li标签-->
        {{if $value.text}}
          <li>{{$value.text}}</li>
        {{/if}}
      {{/each}}
    </ul>
```

### JS代码的编写

```javascript
var obj = {
    list: [
      { title: '2016.11.25' },
      { text: '这是第一句', name: 'jack' },
      { text: '这是第二句', name: 'tom' },
      { text: '这是第三句', name: 'lili' }
    ]
  }
//template传入的数据必须是一个对象，然后到模板里它会默认点一下那个list属性
  var tem = template( 'myTemplate', obj )
  document.getElementById( 'box' ).innerHTML = tem;
```

### 结果为：

![Aaron Swartz](http://ww1.sinaimg.cn/large/005QDhBjgw1fa4g6cpcipj30oo0b8wf5.jpg)

