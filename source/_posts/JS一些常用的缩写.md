---
title: JS一些常用的缩写
date: 2017-11-01 10:50:31
tags:

---

1.for循环

```javascript
var arr = [1,2,3,4];
for(var i= 0; i< arr.length; i++) {
    console.log(arr[i]);
}
```

等同于：

```javascript
for(var item of arr) {
  console.log(item);
}
```

2.十进制数

```javascript
setTimeout(function(){
  console.log('done');
},1000);
```

等同于：

```javascript
setTimeout(function(){
  console.log('done');
},1e3);
```

1. Array.find

```javascript
const pets = [
    { type: 'Dog', name: 'Max'},
    { type: 'Cat', name: 'Karl'},
    { type: 'Dog', name: 'Tommy'},
]
function findDog(name) {
    for(let i = 0; ii) {
    if(pets[i].type === 'Dog' && pets[i].name === name) {
    return pets[i];
    }
}
}
```

等同于：

```javascript
pet = pets.find(pet => pet.type ==='Dog' && pet.name === 'Tommy');
console.log(pet); // { type: 'Dog', name: 'Tommy' }
```

4.双位操作符

```javascript
Math.floor(4.9) === 4  //true
```

可以使用双位运算符代替上面的写法：

```javascript
~~4.9 === 4  //true
```

