---
title: JS学习笔记-算法
date: 2018-05-15 10:10:46
tags:
---

**1.二维数组的全排列组合**

> 如输入[[1,2],[3,4],[5,6]] 
>
>   输出： 
>
>   [ 1, 3, 5 ] 
>
>   [ 1, 3, 6 ] 
>
>   [ 1, 4, 5 ] 
>
>   [ 1, 4, 6 ] 
>
>   [ 2, 3, 5 ] 
>
>   [ 2, 3, 6 ] 
>
>   [ 2, 4, 5 ] 
>
>   [ 2, 4, 6 ]

```javascript
function printArr(arr,n,res){
    for(var i = 0; i<arr[i].length;i++){
        if(n == 0){
            res = []
        }
        if(n<arr.length){
            var _res = res.slice()
            _res.push(arr[n][i])
            if(n == arr.length-1){
                console.log(_res)
            }else{
                printArr(arr,n+1,_res)
            }
        }
    }
}
// 测试：
var arr = [[1,2],[3,4],[5,6]]
printArr(arr,0);
```

**2.打印青蛙跳台阶的所有方式**

> 注意 不是求方式的个数，而是打印每种情况   台阶数为10，每次跳1格或两格

```javascript
function step(n,res){
    if(n==0){
        res=[]
    }
    var i=1
    while(i<3){
        if(n+i<=10){
            var _res = res.slice()
            _res.push(i)
            if(n+i == 10) {
                console.log(_res)
            }else{
                step(n+i, _res)
            }
        }
        i++
    }
}
step(0)
```

**3.统计一个字符串出现最多的字母**

> 输入 ： afjghdfraaaasdenas 
>
> 输出：a

```javascript
function findMaxDuplicateChar(str) {
    if (str.length == 1) {
        return str;
    }
    var charObj = {};
    for (var i = 0; i < str.length; i++) {
        if (!charObj[str.charAt(i)]) {
            charObj[str.charAt(i)] = 1;
        } else {
            charObj[str.charAt(i)] += 1;
        }
    }
    var maxChar = '',
        maxValue = 1;
    for (var k in charObj) {
        if (charObj[k] >= maxValue) {
            maxChar = k;
            maxValue = charObj[k];
        }
    }
    return maxChar;
}
// 测试：
findMaxDuplicateChar('afjghdfraaaasdenas')
```

#### 排序

排序算法有很多种，而经典的和用的比较多的就是冒泡和快排，[动画演示](http://jsdo.it/norahiko/oxIy/fullscreen)

**4.冒泡排序**

> 输入 ： [33,1,4,23,12,43,54,34]
>
> 输出：[1, 4, 12, 23, 33, 34, 43, 54]

```javascript
function bubble(arr) {
    for(var i= 0; i< arr.length -1; i++) {
        for(var j= i; j< arr.length -1; j++) {
            if(arr[j] > arr[j+1]) {
                var tmp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = tmp;
            }
        }
    }
    return arr;
}
//测试:
console.log(bubble([33, 1, 4, 23, 12, 43, 54, 34]));
```

**5.快速排序**

> 输入 ： [33,1,4,23,12,43,54,34]
>
> 输出：[1, 4, 12, 23, 33, 34, 43, 54]

```javascript
function quick(arr) {
    if (arr.length <= 1) {
        return arr;
    }
    var middleIndex = Math.floor(arr.length / 2);
    var middle = arr.splice(middleIndex, 1)[0];
    var leftArr = [],
        rightArr = [];
    for (var i = 0; i < arr.length; i++) {
        if (arr[i] < middle) {
            leftArr.push(arr[i]);
        } else {
            rightArr.push(arr[i]);
        }
    }
    return quick(leftArr).concat([middle], quick(rightArr));
}

console.log(quick([33,1,4,23,12,43,54,34]));
```

**6.生成指定范围内的随机数**

> 输入 ： 5，10
>
> 输出：5，7，9，8，6，10…...

```javascript
function randomNum(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

setInterval(function(){
    console.log(randomNum(5, 10));
},1000)
```

