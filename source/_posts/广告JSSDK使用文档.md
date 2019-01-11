## 广告JSSDK使用文档

### 基本介绍

JSSDK适用于移动端H5页面

### 使用方法

#### 第一步：引入相关资源

```javascript
<script src="http://pre.adbaitai.com/download/jssdk/btmjssdk.min.js"></script>
    //支持https,路径和http一样
    //尽量不要动态加载JS资源文件，容易被浏览器广告屏蔽插件过滤
```

#### 第二步:调用请求广告的JS方法(需要放在页面底部或者放在onload中)

```javascript
              btmAd({
                  container: '.btm-media-container', //需要将广告放入的位置
                  pid:'4_6_9',
                  app_key: 'xinghun-ceshi-1', //媒体app_key
                  app_secret:'1',
                  target: '_blank', //广告的打开方式
                  success: function(res){
                    //广告成功加载后的回调方法
                      console.log(res);
                  },
                  error: function(data) {
                    //广告成功加载后的回调方法
                    console.log(data);
                  }
              })
```

### 调用参数说明

| 名称         | 必填   | 定义       | 类型       | 默认     | 说明                                       |
| ---------- | ---- | -------- | -------- | ------ | ---------------------------------------- |
| container  | 否    | 广告位容器    | String   | body   | 需要将广告放入的位置                                            浮标广告为悬浮广告，填写则放入指定位置，不填写则放在默认位置                                                    插屏广告为悬浮广告，不需要填写，填写无效 |
| app_key    | 是    | 应用Key    | String   | -      | -                                        |
| pid        | 是    |          | String   | -      | -                                        |
| app_secret | 是    |          | String   | -      | -                                        |
| target     | 否    | 广告页面打开方式 | String   | _blank | _self:本页面打开_blank:新页面打开_top:顶级窗口打开       |
| success    | 否    | 请求成功回调函数 | Function | -      | 回调数据的所有参数可打印查看,如:success:function(res){                               console.log(res)                                                         } |
| error      | 否    | 请求失败回调函数 | Function | -      | -                                        |

### 回调参数说明[基础参数]

| 名称        | 类型     | 定义   |
| --------- | ------ | ---- |
| picUrl    | String | 广告图片 |
| pitType   | String | 广告类型 |
| skipUrl   | String | 广告链接 |
| pitWidth  | Number | 图片宽度 |
| pitHeight | Number | 图片高度 |

### 自定义广告高级用法说明

此方法只有非标准的多条素材的自定义广告需要使用，单条素材的自定义广告和标准广告一样支持渲染

多条素材的自定义广告与标准广告有所区别，多条素材的自定义广告只提供数据，并不做任何界面渲染，界面渲染部分由媒体方根据兑吧提供的数据自行定制

示例：

```html
    <div id="btm-media-container">
        <img />
    </div>
```

```javascript
    TuiaMedia({
      pid:'4_6_9',
      app_key: 'xinghun-ceshi-1', //媒体app_key
      app_secret:'1',
      success:function(res){
        console.log(res)
        //将广告图片放入到网页中
        document.querySelector('#tuia-media-container img') 		      .setAttribute('src',res.material_list[0].image_url); 
      }
    })
```

### 更新日志

1.2.0

优化JS包体积大小

1.1.0

添加了开屏，banner和自定义的广告形式

1.0.0

JSSDK诞生

