---
title: onMessage和sendMessage
date: 2017-08-20 10:58:45
tags:

---

之前一直用vue和react框架去开发，对于数据的传递都是通过Prop的方式或者是申明一个事件然后在需要的地方触发一下，数据比较庞大的时候，同时也需要多个组件之间进行共享，vuex, redux 也是不错的选择。

（`子===>>父`传递）但是有种场景，数据不是很大，然后只是个别数据需要多个组件之间共享，如果因为这个需求就引入Vuex,redux那就太小题大做了，传统的方式就是逐层上传，然后通过有关联的父组件进行向下传递，直到目标组件，可以是可以，就是想想都不想写。。。

这个时候就需要全局的方法，去触发和监听：

```javascript
window.onMessage = function(name,callback){
            var msgs = window.onMessage[name] = window.onMessage[name] || [];
            if(msgs.indexOf(callback)===-1){
                msgs.push(callback);
            }
        };

        window.sendMessage = function(name,param){
            var msgs = window.onMessage[name] || [];
            msgs.forEach(function(msg){
                msg(param);
            })
        };

        window.removeMessage = function(name,callback){
            if(callback && window.onMessage[name]){
                window.onMessage[name].forEach(function(fun,index){
                    if(fun===callback){
                        window.onMessage[name].splice(index,1);
                    }
                })
            }else{
                delete window.onMessage[name];
            }
        };
```

监听的方式：

```javascript
 window.onMessage( 'st', function(data){
     console.log(data)
    });
    window.onMessage( 'st', function(data){
        console.log(data)
    });
    window.sendMessage('st', '111');
```

控制台打印结果：

![](http://ww1.sinaimg.cn/large/005QDhBjgy1fiq0m57ycfj314m076gmd.jpg)