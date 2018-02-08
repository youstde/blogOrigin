---
title: getBoundingClientRect获取的top和offsetTop获取的top区别
date: 2017-10-23 16:15:01
tags:

---

```css
* {margin: 0; padding:0;}
		#main {
			width: 100%;
			height: 2000px;
			background-color: #ccc;
			overflow: hidden;
		}
		#box {
			width: 200px;
			height: 150px;
			margin-top: 300px;
			background-color: red;
			border: 3px solid pink;
		}
```

```Html
<div id='main'>
		<div id='box'></div>		
	</div>
```

```javascript
		// console.log(document.body.scrollHeight);
		// console.log('offsetHeight:',document.body.offsetHeight);
		// console.log('clientHeight:',document.body.clientHeight);
		var box = document.getElementById('box');
		console.log('offsetTop:',box.offsetTop);
		console.log('clientTop:',box.clientTop);
		console.log('getBoundingClientRect:',box.getBoundingClientRect().top);
		box.onclick = function(e) {
			console.log('clientX:',e.clientX);
			console.log('clientY:',e.clientY);
			console.log('offsetX:',e.offsetX);
			console.log('offsetY:',e.offsetY);
			//clientX,offsetX都是相对于可视区域去定位的
			// box.offsetTop获取的是该元素到文档顶部的距离，包括滚动上去的部分
			// 而box.getBoundingClientRect().top是相对于可视区域的高度
		}
```

