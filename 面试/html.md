### `html语义化`

​	用对应的标签去写对应的内容，比如h标签，p段落...

​		让人更容易读懂（可读性）

​		让搜索引擎更容易读懂(SEO)



### `块元素 & 内联元素`

* div h1 h2 table ul ol p
* span img input button



### `mata viewport 是做什么的，怎么写`

目的：为了移动端不让用户缩放使用的

* width = device-width
  * 将布局视窗的宽度设置为设备分辨率的宽度
* initial-scale = 1
  * 页面初始缩放比例为屏幕分辨率

* maxmum-sacle = 1
  * 指定用户缩放的最大比例

* Minmum-scale = 1
  * 指定用户缩放的最小比例



### `行内元素有哪些？块级元素有哪些？空(void)元素有那些？`

常用的块状元素有：`<div>、<p>、<h1>...<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>`

常用的内联元素有：`<a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>`

> 常用的内联块状元素有:`<img>、<input>`



### `iframe有哪些缺点？`

`iframe的优点`：

* 原封不动的把嵌入的网页展现出来

* 如果有多个网页引用iframe，那么你只需要修改iframe的内容，就可以实现调用的每一个页面内容的更改，方便快捷。

* 用iframe来嵌套，可以增加代码的可重用。

`iframe的缺点：`

* 会产生很多页面，不容易管理

* 如果框架个数多的话，可能会出现上下、左右滚动条，会分散访问者的注意力，用户体验度差

* 代码复杂，无法被一些搜索引擎索引到，不利于搜索引擎优化

* 移动设备兼容性差

* 增加http请求



### `script标签为什么要放在body标签的底部，【defer async】`

因为浏览器在渲染html的时候，从上到下依次执行，遇到js文件就会停止当前页面的渲染，转而去下载js文件，如果将script标签放在头部，如果文件又很大的情况下，首屏时间就会延长，影响用户体验。解决方法：

`script标签 defer\async的区别` 首先都是`让js文件能够异步下载，不阻塞页面的渲`, 区别就是`defer必须等待整个文档渲染完成后才执行` 而`async在下载完成后，会暂停html的解析，转去执行js`

#### 定义和用法

defer 属性规定当页面已完成加载后，才会执行脚本。

**注释：**defer 属性仅适用于外部脚本（只有在使用 src 属性时）。

**注释：**有多种执行外部脚本的方法：

- 如果 async="async"：脚本相对于页面的其余部分异步地执行（当页面继续进行解析时，脚本将被执行）
- 如果不使用 async 且 defer="defer"：脚本将在页面完成解析时执行
- 如果既不使用 async 也不使用 defer：在浏览器继续解析页面之前，立即读取并执行脚本

```html
<script type="text/javascript" src="demo_defer.js" defer="defer"></script>
```



### `src 和 ref 的区别`

href是超文本引用，它指向资源的位置，建立与目标文件之间的联系

src目标是把资源下载到页面中 浏览器解析方式 href 不会阻塞对文档的处理（这就是官方建议使用link引入而不是@import） src 会阻塞对文档的处理



### `DOCTYPE 标签`

防止浏览器在渲染文档时，切换到我们称为“怪异模式(兼容模式)”的渲染模式。“",确保浏览器按照最佳的相关规范进行渲染，而不是使用一个不符合规范的渲染模式。

Doctype声明于文档最前面，告诉浏览器以何种方式来渲染页面，这里有两种模式，严格模式和混杂模式。

严格模式的排版和JS 运作模式是 以该浏览器支持的最高标准运行。



### `<img>的title和alt有什么区别`

- 通常当鼠标滑动到元素上的时候显示
- `alt`是`<img>`的特有属性，是图片内容的等价描述，用于图片无法加载时显示、读屏器阅读图片。可提图片高可访问性，除了纯装饰图片外都必须设置有意义的值，搜索引擎会重点分析。



### `HTTP的几种请求方法用途`

- `GET`方法
  - 发送一个请求来取得服务器上的某一资源
- `POST`方法
  - 向`URL`指定的资源提交数据或附加新的数据
- `PUT`方法
  - 跟`POST`方法很像，也是想服务器提交数据。但是，它们之间有不同。`PUT`指定了资源在服务器上的位置，而`POST`没有
- `HEAD`方法
  - 只请求页面的首部
- `DELETE`方法
  - 删除服务器上的某资源
- `OPTIONS`方法
  - 它用于获取当前`URL`所支持的方法。如果请求成功，会有一个`Allow`的头包含类似`“GET,POST”`这样的信息
- `TRACE`方法
  - `TRACE`方法被用于激发一个远程的，应用层的请求消息回路
- `CONNECT`方法
  - 把请求连接转换到透明的`TCP/IP`通道



### `HTTP状态码及其含义`

`1XX`：信息状态码

- `100 Continue` 继续，一般在发送`post`请求时，已发送了`http header`之后服务端将返回此信息，表示确认，之后发送具体参数信息

`2XX`：成功状态码

- `200 OK` 正常返回信息
- `201 Created` 请求成功并且服务器创建了新的资源
- `202 Accepted` 服务器已接受请求，但尚未处理

`3XX`：重定向

- `301 Moved Permanently` 请求的网页已永久移动到新位置。
- `302 Found` 临时性重定向。
- `303 See Other` 临时性重定向，且总是使用 `GET` 请求新的 `URI`。
- `304 Not Modified` 自从上次请求后，请求的网页未修改过。

`4XX`：客户端错误

- `400 Bad Request` 服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
- `401 Unauthorized` 请求未授权。
- `403 Forbidden` 禁止访问。
- `404 Not Found` 找不到如何与 `URI` 相匹配的资源。

`5XX`:服务器错误

- `500 Internal Server Error` 最常见的服务器端错误。
- `503 Service Unavailable` 服务器端暂时无法处理请求（可能是过载或维护）。



### 请描述一下 `cookies`，`sessionStorage` 和 `localStorage` 的区别？

- `cookie`是网站为了标示用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）
- cookie数据始终在同源的http请求中携带（即使不需要），记会在浏览器和服务器间来回传递
- `sessionStorage`和`localStorage`不会自动把数据发给服务器，仅在本地保存
- 存储大小：
  - `cookie`数据大小不能超过4k
  - `sessionStorage`和`localStorage`虽然也有存储大小的限制，但比`cookie`大得多，可以达到5M或更大
- 有期时间：
  - `localStorage` 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据
  - `sessionStorage` 数据在当前浏览器窗口关闭后自动删除
  - `cookie` 设置的`cookie`过期时间之前一直有效，即使窗口或浏览器关闭



### `html5有哪些新特性、移除了那些元素？`

- `HTML5` 现在已经不是 `SGML` 的子集，主要是关于图像，位置，存储，多任务等功能的增加
  - 新增选择器 `document.querySelector`、`document.querySelectorAll`
  - 拖拽释放(`Drag and drop`) API
  - 媒体播放的 `video` 和 `audio`
  - 本地存储 `localStorage` 和 `sessionStorage`
  - 离线应用 `manifest`
  - 桌面通知 `Notifications`
  - 语意化标签 `article`、`footer`、`header`、`nav`、`section`
  - 增强表单控件 `calendar`、`date`、`time`、`email`、`url`、`search`
  - 地理位置 `Geolocation`
  - 多任务 `webworker`
  - 全双工通信协议 `websocket`
  - 历史管理 `history`
  - 跨域资源共享(CORS) `Access-Control-Allow-Origin`
  - 页面可见性改变事件 `visibilitychange`
  - 跨窗口通信 `PostMessage`
  - `Form Data` 对象
  - 绘画 `canvas`
- 移除的元素：
  - 纯表现的元素：`basefont`、`big`、`center`、`font`、 `s`、`strike`、`tt`、`u`
  - 对可用性产生负面影响的元素：`frame`、`frameset`、`noframes`
- 支持`HTML5`新标签：
  - `IE8/IE7/IE6`支持通过`document.createElement`方法产生的标签
  - 可以利用这一特性让这些浏览器支持`HTML5`新标签
  - 浏览器支持新标签后，还需要添加标签默认的样式
- 当然也可以直接使用成熟的框架、比如`html5shim`

**如何区分 HTML 和 HTML5**

- `DOCTYPE`声明、新增的结构元素、功能元素