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

