---
title: JS学习笔记-get和set
date: 2017-08-01 11:08:10
tags:
---

现在有个对象，我想在外界去获取该对象的属性或者修改属性的时候我能检查和控制(不直接修改到对象上的属性)，那么get和set是时候该登场了

直接上代码：

```javascript
 var p = {
        name:"chen",
        work:function() {
            console.log("wording...");
        },
        _age:18,
        get age(){
            return this._age;
        },
        set age(val) {
            console.log('val:'+val);
            if (val<0 || val> 100) {//如果年龄大于100就抛出错误
                throw new Error("invalid value");
            }else{
                this._age = val;
            }
        }
    };
```

通过上面的代码，当我通过**p.age**去获取的时候，返回的是对象上**_age**的值，通过**p.age = 20**去修改的时候，其实走的是**set age**这个方法，这样我就可以对这个过程有所控制。

在**vue**中，其实也有很多这种设计，我们也可以在vue中去写类似的这种

```javascript
//这是一个components片段
var uploadimg = {
    template:tpl,
    props:["src"],
    computed:{
        imgsrc:{
            get:function(){
                return this.src;
            },
            set:function(result){
                this.src=result;
            }
        }
    },
    methods:{
        change:function(event){
            var file = event.target.files[0];
            var reader = new FileReader();
            reader.onload = function() {
                this.imgsrc=reader.result;
                this.$emit('listenmsgfromchild',this.imgsrc);
            }.bind(this);
            reader.readAsDataURL(file);
        }
    }
};
```

上面代码中，src是父元素传过来的，而此时我在**tpl**这个template中去设置了一个计算属性**imgsrc**，此时是一个对象，而不是一个返回函数，传统的计算属性是这样写的：

```javascript
props: ["src"],
computed:{
        imgsrc () {
          return this.src;
        }
    }
```

但是通过对象的那种方式，在获取imgsrc这个计算属性的时候是可以对这个过程进行控制的