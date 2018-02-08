---
title: 小程序scroll-view高度问题
date: 2016-11-22 16:01:55
tags:
---

### 使用scroll-view时，如果不给它一个具体高度，`bindscrolltolower`事件就触发不了

## wxml

```html
 <scroll-view scroll-y = 'true' class = 'content' style=' height:{{winH}}px' bindscrolltolower = 'ss'>
    <view class = 'header'>
        <text>图书{{totaleRecord}}本图书</text>
    </view>
    <view class = 'common-list'>
        <block wx:for="{{pageData}}">
        <view class="list-item" data-bid="{{item.id}}" bindtap="toDetailPage">
          <view class="index-list-item">
            <view class="cover">
              <image class="cover-img" src="{{item.image}}"></image>
            </view>
            <view class="content">
              <view class="title">{{item.title}}</view>
              <text class="desc">{{item.rating.average == '0.0' ? '无' : item.rating.average}}/<block wx:for="{{item.author}}" wx:for-item="it" wx:key="*this">{{it}}/</block>{{item.pubdate}}</text>
            </view>
          </view>
        </view>
      </block>
    </view>
 </scroll-view>
```

## JS

```javascript
onLoad: function(){
        var _this = this;
        wx.getSystemInfo({
          success: function(res) {
            _this.setData({
              //获取到高度
                winH: res.windowHeight - (100 * res.windowWidth / 750) ,
                winW: res.windowWidth
            })
          }
        })
    }
```

