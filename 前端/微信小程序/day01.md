### 配置

`app.json` 全局配置

`页面名.json` 页面配置（页面配置权重高于全局配置，相同配置，页面会覆盖全局)

[全局配置 | 微信开放文档 (qq.com)](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html)

### 样式

`app.wxss`: 全局样式

`page.wxss`: 页面样式（权重更高）



### 自定义组件

注册自定义组件(组件page文件)

```json
 {
   "component": true
 }
```

引入

```json
{
	"usingComponents": {
    "tab-bar": "/components/tabBar/tabBar"
  }
}
```



### 定义方法

对局主页面定义的方法直接写在page对象内

```javascript
<tab-bar bindmyevent="click"></tab-bar>

const app = getApp();
Page({
	click: function(e) {
	
	}
})
```

但对与组件定义的方法就要写在methods中了

```javascript
<view class="tab-con" bindtap="click">
  
methods: {
    click() {
      var my
      this.triggerEvent('myevent', {
        person1: '张三',
        person2: '李四'
      }, {
        bubbles: false, //事件是否冒泡
        composed: false,//事件是否穿越组件边界
        capturePhase: false //事件是否冒泡
      })
    }
  }
```



### 组件通信

子组件向父组件通信：子组件发送自定义事件 父组件接收 从而达到数据通信的目的

```javascript
<view class="tab-con" bindtap="click"> //绑定一个事件
 
  
 //事件触发后传递数据
 click() {
      var my
      this.triggerEvent('myevent', { //myevent 自定义事件 绑定到父组件  第二个参数为对象传递到自定义事件中
        person1: '张三',
        person2: '李四'
      }, {
        bubbles: false, //事件是否冒泡
        composed: false,//事件是否穿越组件边界
        capturePhase: false //事件是否冒泡
      })
    }

//父组件接收
<tab-bar bindmyevent="click"></tab-bar> //使用bind 绑定自定义事件

 click: function(e) {
    console.log(e.detail); //e.detail就是子组件传递的参数(对象)
  },

```

