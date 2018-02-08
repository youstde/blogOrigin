---
title: vue系列之componet的全局注册和use
date: 2017-12-01 14:25:51
tags:
---

## `写在前面`

我现在写了一个组件svgIcon，想注册为一个全局组件，我只说我平时用的一种：`Vue.component('svg-icon', SvgIcon)`，这样我们就可以在任何组件里直接使用就可以了，而不需要再在每个需要用到该组件的地方引入了。



## `vue.use()`

平时我们需要用到别人的插件，首先是`install`，然后在主JS中（main.js）中引入，然后使用`vue.use(插件)`挂载一下就可以使用了。

webpack首先会加载main.js，所以我们在main的js里面引入。我以element ui来做对比说明

```vue
import Vue from 'vue'
import App from './App.vue'
 
// 引入element-ui组件
import ElementUi from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
 
// 引入自定义组件。index.js是组件的默认入口
import Loading from '../components/loading'
Vue.use(Loading);
 
Vue.use(ElementUi);
new Vue({
 el: '#app',
 render: h => h(App)
})
```

然后在Loading.vue里面定义自己的组件模板

```vue
<!-- 这里和普通组件的书写一样 -->
<template>
  <div class="loading">
    loading...
  </div>
</template>
```

在index.js文件里面添加install方法

```javascript
import MyLoading from './Loading.vue'
// 这里是重点
const Loading = {
  install: function(Vue){
    Vue.component('Loading',MyLoading)
  }
}
 
// 导出组件
export default Loading
```

接下来就是在App.Vue里面使用组件了，这个组件已经在main.js定义加载了

```vue
<template>
 <div id="app">
 <!-- 使用element ui的组件 -->
 <el-button>默认按钮</el-button>
 
 <!-- 使用自定义组件 -->
 <Loading></Loading>
 </div>
</template>
```

