---
title: 面试中的问题CSS
date: 2017-11-28 10:25:53
tags:
---

## `垂直居中`

1.使用定位

```css
position: absolute;
left: 50%;
top: 50%;
transform:translateY(-50%);
```

2.table布局

```html
<div class="something-semantic">
   <div class="something-else-semantic">
       Unknown stuff to be centered.
   </div>
</div>
```

```css
.something-semantic {
   display: table;
   width: 100%;
}
.something-else-semantic {
   display: table-cell;
   text-align: center;
   vertical-align: middle;
}
```

3.flex布局

```css
.box2{
    display: flex;
    justify-content:center;
    align-items:Center;
}
```

