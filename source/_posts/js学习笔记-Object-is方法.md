---
title: js学习笔记-Object.is方法
date: 2017-07-28 15:25:54
tags:

---

**Object.is(x, y)**判断x 和 y是否相同

```javascript
Object.is(1, 1)   //true
Object.is([], [])  //false
```

如果Object.is()的封装：

```javascript
 //Object.is
    console.log(_objectIs('a', 'a'));
    function _objectIs (a, b) {
        if (!Object.is) {
            Object.is = function(x, y) {
                // SameValue algorithm
                if (x === y) { // Steps 1-5, 7-10
                    // Steps 6.b-6.e: +0 != -0
                    return x !== 0 || 1 / x === 1 / y;
                } else {
                    // Step 6.a: NaN == NaN
                    return x !== x && y !== y;
                }
            };
        }
        return Object.is(a, b);
    }
```

