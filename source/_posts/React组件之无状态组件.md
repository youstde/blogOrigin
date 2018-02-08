---
title: React组件之无状态组件
date: 2017-08-23 13:56:12
tags:

---

![](http://ww1.sinaimg.cn/large/005QDhBjgy1fitmchh0isj317n0u07bm.jpg)

目前，react组件有三种写法，分别是es5的createClass写法，es6的class写法，以及stateless(无状态组件)写法

但是es5的写法已经渐渐的退出了历史的舞台。

es5写法：

```javascript
var AppComponent = React.createClass({

   render:function(){
      return (
       <div> 返回值最外层必须是闭合标签  </div>
     )
   }
})
```

现在主流的方法是es6的写法，class组件和stateless组件互补的写法

这两种组件通常要搭配使用，互为替补，这是react组件最好的应用方式。

在实践前，先来讲讲两个概念，分别是——容器组件，展示组件。 也就是Container Component和 Presentational Component

所谓Container Component（容器组件），简单来讲就是它通常是作为一个父组件，它底下领着一群子组件。容器组件（父组件）的作用在于，给子组件传递数据，并且定义逻辑方法传递给子组件。 子组件不参与或少参与业务逻辑处理，它主要负责接收和展示容器组件传递过来的数据，以及调用传递过来的逻辑方法。 而这里所说的子组件，通常就是展示型组件，也就是Presentational Component。

这里为什么说class组件和stateless组件搭配使用，互为替补，是react组件的最好应用方式呢？

首先，stateless组件没有生命周期，无实例化，性能最好。而展示组件通常只需要做数据展示，和逻辑方法调用，它并不需要使用到生命周期方法。 这不正和stateless组件最为契合吗？ 于是，stateless组件，通常用作于展示组件。

再来说说class组件，它有生命周期，再搭配es6/7语法，它可以处理众多复杂的业务逻辑。 而容器组件通常只专注于处理业务逻辑，需要使用生命周期，对于数据的展示则交给展示组件。这正和class组件最为契合。于是，通常class组件作为容器组件。

这两种组件，各自分工，互为替补，是react组件最好的应用方式。

下面是一个例子：

```jsx
# 容器组件
// /containers/index.js

import ListTableComponent from '/components/ListTable';
import ItemTableComponent from '/components/ItemTable';

class AppComponent extends React.Component {

   state = {
        loadding: false
   }


render =()=>{

   const { list, item } = this.props;  

   const handdleLoad = ()=>{

     // ToDo
  }

   const listProps = { list, handdleLoad }; 

   const  itemProps = { item, handdleLoad }; 

   return <div className="box">

      <ListTableComponent       // 子组件
       { ...listProps }                // ‘...’ 是es7的展开属性运算符，
      />

      <ItemTableComponent    // 子组件
       { ...itemProps }
     />

   </div>
 }
}

# 展示组件一 
//  /components/ListTable.js

const ListTable = ({ list,handdleLoad })=>{


   return <div>

           ……
           <td> {list.coutry} </td>
           <td> {list.address} </td>
          ……

      <button onClick={ handdleLoad() }> 做一些事情 </button>
    </div>
}

# 展示组件二
//  /components/ItemTable.js

const ItemTable = ({ item,handdleLoad })=>{


   return <div>

           ……
           <td> {item.name} </td>
           <td> {item.age} </td>
          ……

      <button onClick={ handdleLoad() }> 做一些事情 </button>
    </div>
}
```

这个例子中，容器组件index.js载入了两个子组件，这两个是展示组件，容器组件定义了handdleLoad方法，并从props拿到数据list和item两个数据源，再组织成listProps和itemProps两个props属性对象，并把他们分别传给ListTable和ItemTable两个展示组件。 这两个展示组件从props中拿到传过来的数据，并在render方法中展示出来，并不需要处理过多业务逻辑。

[`点我去原文`](https://zhuanlan.zhihu.com/p/26216173)

