### 1.垂直居中

```css
//方式一
.parent {
  display: table;
}
.son {
  display: table-cell;
  vertical-align: middle;
}
//方式二
.son {
    position: absolute;
    top: 50%;
    transform: translate( 0, -50%);
}
//方式三
.son {
    position: absolute;
    top: 50%;
    height: 高度;
    margin-top: -0.5高度;
}
//方式四
.son {
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto 0;
}
```



### 2.清除浮动

**clear清除**

```css
.clearfix:after {
    content: '.';
    height: 0;
    display: block;
    clear: both;
}
```



**原理：**

`after`伪元素告诉浏览器：我左右两边都不能有浮动元素，浏览器为了满足这个要求，会把伪元素放置在浮动元素之下，父元素的高度也被支撑起来

**overflow**

```css
overflow: auto
```



### 3.块格式化上下文(BFC)

> 具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。
>
> 通俗一点来讲，可以把 BFC 理解为一个封闭的大箱子，箱子内部的元素无论如何翻江倒海，都不会影响到外部



下列方式会创建**块格式化上下文**：

- 根元素（`<html>）`
- 浮动元素（元素的 [`float`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float) 不是 `none`）
- 绝对定位元素（元素的 [`position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position) 为 `absolute` 或 `fixed`）
- 行内块元素（元素的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `inline-block`）
- 表格单元格（元素的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `table-cell`，HTML表格单元格默认为该值）
- 表格标题（元素的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `table-caption`，HTML表格标题默认为该值）
- 匿名表格单元格元素（元素的 [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `table、``table-row`、 `table-row-group、``table-header-group、``table-footer-group`（分别是HTML table、row、tbody、thead、tfoot 的默认属性）或 `inline-table`）
- [`overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow) 计算值(Computed)不为 `visible` 的块元素
- [`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 值为 `flow-root` 的元素
- [`contain`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain) 值为 `layout`、`content `或 paint 的元素
- 弹性元素（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `flex` 或 `inline-flex `元素的直接子元素）
- 网格元素（[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display) 为 `grid` 或 `inline-grid` 元素的直接子元素）
- 多列容器（元素的 [`column-count`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/column-count) 或 [`column-width` (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/column-width) 不为 `auto，包括 ``column-count` 为 `1`）
- `column-span` 为 `all` 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中



**. BFC 可以包含浮动的元素（清除浮动）**

如果使触发容器的 BFC，那么容器将会包裹着浮动元素。

```css
.box {
           border: 1px solid red;
           display: flow-root; //创建BFC
       } 
       .son {
           width: 100px;
           height: 100px;
           background-color: royalblue;
           float: left;
       }
```



### 4.盒模型

**标准盒模型**

`width = content`



**(IE)盒模型**

box-sizing: border-box

`width = content + border + padding  `



**offsetWidth**

offsetWidth = (内容宽度 + 内边距 + 边框), 无外边距

**clientWidth**

clientWidth = 内容宽度 + 内边距



### margin 负值

* margin-top 和 margin-left 负值, 元素向上，向左移动
  * margin-right 负值， 右侧元素向左移动，自身不受影响
* margin-bottom 负值，下方元素向上移，自身不受影响



### 圣杯布局

两边固定，中间自适应

float实现（flex也可以实现）

```html

    <style>
        header {
            width: 100%;
            height: 100px;
            background-color: rebeccapurple;
        }
        .container {
            width: 100%;
            height: 500px;
        }
        .cloumn {
            float: left;
        }
        .container {
            box-sizing: border-box;
            padding-left: 120px;
            padding-right: 120px;
        }
       .container .cloumn {
           float: left;
           height: 100%;
       }
       .right {
           margin-right: -120px;
       }
       .left {
           margin-left: -120px;
       }
        .center {
            background-color: royalblue;
            width: 100%;
        }
        footer {
            height: 100px;
            background-color: salmon;
        }
    </style>
</head>
<body>
    <header></header>
    <div class="container">
        <div class="left cloumn"> left </div>
        <div class="center cloumn"> center </div>
        <div class="right cloumn"> right </div>
    </div>
    <footer></footer>
</body>
```

<img src="https://cdn.jsdelivr.net/gh/player-404/picture/%E5%8F%8C%E9%A3%9E%E7%BF%BC%E5%B8%83%E5%B1%80.png" width="100%" />



### absoulte&relative相对于什么定位

absoulte相对于最近一层定位元素定位

relative相对于自身定位



### 水平居中

inline元素： text-align: center

block元素： margin: auto

absolute元素：left50% + margin-left: -50%

flex: justify-center



### rem是什么

rem是长度单位， 相对于根元素字体大小

px 绝对长度单位

em 相对长度单位， 相对于父元素子日大小

**响应式布局常用解决方案**

media-query(媒体查询)， 根据不同的屏幕宽度设置跟元素 font-size再设置相应的rem



