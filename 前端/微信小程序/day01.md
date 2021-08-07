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





