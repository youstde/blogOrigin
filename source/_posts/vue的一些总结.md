---
title: vue的一些总结
date: 2017-01-19 09:27:47
tags:
---

### 1.路由激活状态（class）的切换

```javascript
const router = new VueRouter({
  'linkActiveClass': 'active',   //这句话就是当路由为选中状态下，它的class
  routes // （缩写）相当于 routes: routes
});
//其中路由的地址为外部引用的，单独写成了一个js文件，这样方便管理
```

这是vue1.0的做法：如果使用v-link去路由时，当点击路由时，active的状态可以显示在外层包裹的**li**上，需要在li里添加**v-link-active**的属性，然后在router中设置**linkActiveClass**就可以了

### 2.class

```javascript
//使用：class的时候可以传数组和对象
当传入的是数组时，数组中的class将都会被添加到class中，
而对象中的，只有当其值为true时才会被添加到class中
```

### 3.vuex

在代码中，可以跨过actions去操作mutations，actions主要是异步请求数据然后传到mutaions中

#### 1.actions

可以去异步操作

#### 2.mutations

只能同步去操作

#### 3.getters

获取store中存储的数据

```javascript
或者通过
import { mapGetters } from 'vuex'
 export default{
        data () {
            return {
                st: '123'
            }
        },
        computed: {
            ...mapGetters({
                articles: 'getArticles',
                hotArticles: 'getHotArticles'
            }),
            mm () {
                return this.st + 'kk'
            }
        },
        methods: {
            ss () {
                console.log(this.mm)
            },
            showId (id) {
              //去分发指定的action，id为传入的参数
                this.$store.dispatch('displayArticleId', id)
            }
        }
    }
```

### 4.v-model

原理是：v-bind和v-on:click的合体

### 5.计算缓存

一般在拿到数据之后还需要计算之后才能展示到页面上的话，比较好的方法是使用计算属性

当然，使用Methods也是可以实现的

借用官方文档的例子是：

```Html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

<script>
	var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
```

使用方法实现：

```html
<p>Reversed message: "{{ reverseMessage() }}"</p>

<script>
	// in component
    methods: {
      reverseMessage: function () {
        return this.message.split('').reverse().join('')
      }
    }
</script>
```

不经过计算属性，我们可以在 **method** 中定义一个相同的函数来替代它。对于最终的结果，两种方式确实是相同的。然而，不同的是**计算属性是基于它的依赖缓存**。计算属性只有在它的相关依赖发生改变时才会重新取值。这就意味着只要 **message**没有发生改变，多次访问 **reversedMessage **计算属性会立即返回之前的计算结果，而不必再次执行函数。

#### 

