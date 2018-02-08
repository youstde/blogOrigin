---
title: vue学习笔记-Filter
date: 2017-03-04 23:08:22
tags:
---

Vue2.0中废弃了angular风格的filterBy方法，使用计算属性取而代之

简单例子：

```html
<input v-model = 'expr' />
<div v-for = 'log in filteredLogs'>
	<a>{{log.src}}</a>
    <span>{{log.content}}</span>
</div>

<script>
  	export default {
      data () {
        return {
          expr: '',
          logs: []
        }
      },
      computed: {
        filteredLogs () {
          var _this = this;
          return this.logs.filter(( logItem )=>{
            return logItem.content.indexOf( _this.expr ) > -1
          })
        }
      }
  	}
</script>
```

