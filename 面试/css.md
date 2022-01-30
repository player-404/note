###  1.垂直居中

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
    content: '';
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



常用的有：

* overflow: 不为none
* position: absoulte或fixed
* display: flex 或 inline-block
* float: 不为none



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

box-sizing: content-box

**(IE)盒模型**

box-sizing: border-box

`width = content + border + padding  `



**offsetWidth**

IE盒模型

offsetWidth = (内容宽度 + 内边距 + 边框), 无外边距

标准盒模型

offsetWidth = content

**clientWidth**

clientWidth = 内容宽度 + 内边距



### 5.margin 负值

* margin-top 和 margin-left 负值, 元素向上，向左移动
  * margin-right 负值， 右侧元素向左移动，自身不受影响
* margin-bottom 负值，下方元素向上移，自身不受影响



### 7.absoulte&relative相对于什么定位

`absoulte(绝对定位):`相对于最近一层定位元素定位

`relative(相对定位):`相对于自身定位

#### `sticky`

粘性定位：当元素在屏幕内表现为position:relative. 当元素要滚动出屏幕时，表现为position: fixed;



### 8.水平居中

inline元素： text-align: center

block元素： margin: 0 auto

absolute元素：left50% + margin-left: -50%自身宽度

flex: justify-center



### 9.rem是什么

rem是长度单位， 相对于根元素字体大小

px 绝对长度单位

em 相对长度单位， 相对于`父元素的字体大小`

**响应式布局常用解决方案**

media-query(媒体查询)， 根据不同的屏幕宽度设置跟元素 font-size再设置相应的rem



### 10.margin纵向重叠

相邻元素的margin-top和margin-bottom会重叠，取两个元素margin值最大的那个

解决办法:

1. 底部元素变为行内盒子(`display: inline-block`);
2. 底部元素设置flot
3. 底部元素的position的值为absolute/fixed

#### 父子元素magin重合问题

解决：

父元素设置：

* 外层元素添加padding

* overflow: hidden

* border: 1px solid transparent

子元素：

* 内层元素绝对定位 postion:absolute:

* 内层元素 加float:left;或display:inline-block;

### 11.css浮动问题

所有元素经过**浮动**变为**行内块元素**



### 12.圣杯布局

```html
<style>
        html, body {
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }

        .container {
            padding: 0 200px;
            overflow: hidden;
            box-sizing: border-box;
            width: 100%;
        }

        .content {
            width: 200px;
            height: 200px;
            background-color: forestgreen;
            width: 100%;
        }

        .left {
            width: 200px;
            height: 200px;
            background-color: deepskyblue;
            margin-left: -100%;
            position: relative;
            left: -200px;
        }

        .right {
            width: 200px;
            height: 200px;
            background-color: red;
            margin-right: -200px;
        }

        .fl {
            float: left;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="content fl"></div>
        <div class="left fl"></div>
        <div class="right fl"></div>
    </div>
</body>
```



圣杯布局主要是中间内容先渲染，并且自适应，所以 content 元素放在最前



### 13.双飞翼布局

```html
<style>
        html,
        body {
            padding: 0;
            margin: 0;
        }

        .center {
            width: 100%;
            height: 200px;
            background-color: gold;
        }

        .c-inner {
            margin: 0 200px;
            height: 100%;
            background-color: hotpink;
        }

        .left {
            width: 200px;
            height: 200px;
            background-color: greenyellow;
            margin-left: -100%;
        }

        .right {
            width: 200px;
            height: 200px;
            background-color: grey;
            margin-left: -200px;
        }

        .fl {
            float: left;
        }
    </style>
</head>

<body>
    <div class="center fl">
        <div class="c-inner"></div>
    </div>
    <div class="left fl"></div>
    <div class="right fl"></div>
</body>
```



### 14.手写clearfix

```css
.clearfix::after {
  content: "";
  display: table;
  clear: both;
  
}
兼容IE
.clearfix {
  zoom: 1;
}
```



### 15.flex

1. 声明的两种方式：`display: flex; ` 块级弹性盒子 和 `display: inline-flex;`行内块级弹性盒子

2. 弹性盒子的排列方向(主轴的方向) : `flex-direction` : `row`、`row-reverse`、 `column`、 `column-reverse`

3. 超出宽度是否换行：`flex-wrap`

4. 控制主轴的排列：`justify-content`

* ``start``

  从行首开始排列。每行第一个元素与行首对齐，同时所有后续的元素与前一个对齐。

* `flex-start`

  从头排列

* `flex-end`

  从尾排列

* `center`

  居中排列

* `left`

  伸缩元素一个挨一个在对齐容器得左边缘，如果属性的轴与内联轴不平行，则`left`的行为类似于`start`。

* `right`

  元素以容器右边缘为基准，一个挨着一个对齐,如果属性轴与内联轴不平行，则`right`的行为类似于`end`。

* `space-between`

  分散排列，从两头开始，相邻元素之间的距离是相等的, 两头没有空隙

* `space-around`

  首尾元素的距离页面边缘的距离是其他相邻元素的间隔的一半，两头有空隙

* `space-evenly`

  flex项都沿着主轴均匀分布在指定的对齐容器中。相邻flex项之间的间距，主轴起始位置到第一个flex项的间距,，主轴结束位置到最后一个flex项的间距，都完全一样。

5. 控制交叉轴：`align-items`（单行）

* `flex-start`

  元素向侧轴起点对齐。

* `flex-end`

  元素向侧轴终点对齐。

* `start`

  The item is packed flush to each other toward the start edge of the alignment container in the appropriate axis.

* `end`

  The item is packed flush to each other toward the end edge of the alignment container in the appropriate axis.

* `center`

  元素在侧轴居中。如果元素在侧轴上的高度高于其容器，那么在两个方向上溢出距离相同。

* `left`

  The items are packed flush to each other toward the left edge of the alignment container. If the property’s axis is not parallel with the inline axis, this value behaves like `start`.

* `right`

  The items are packed flush to each other toward the right edge of the alignment container in the appropriate axis. If the property’s axis is not parallel with the inline axis, this value behaves like `start`.

* `self-start`

  The items is packed flush to the edge of the alignment container of the start side of the item, in the appropriate axis.

* `self-end`

  The item is packed flush to the edge of the alignment container of the end side of the item, in the appropriate axis.

* `baseline`
  first baselinelast baseline

  所有元素向基线对齐。侧轴起点到元素基线距离最大的元素将会于侧轴起点对齐以确定基线。

* `stretch`

  弹性元素被在侧轴方向被拉伸到与容器相同的高度或宽度。

6. 控制交叉轴：`align-content`(多行)

- `start`

  所有行从容器的起始边缘开始填充。

- `end`

  所有行从容器的结束边缘开始填充。

- `flex-start`

  所有行从垂直轴起点开始填充。第一行的垂直轴起点边和容器的垂直轴起点边对齐。接下来的每一行紧跟前一行。

- `flex-end`

  所有行从垂直轴末尾开始填充。最后一行的垂直轴终点和容器的垂直轴终点对齐。同时所有后续行与前一个对齐。

- `center`

  所有行朝向容器的中心填充。每行互相紧挨，相对于容器居中对齐。容器的垂直轴起点边和第一行的距离相等于容器的垂直轴终点边和最后一行的距离。

- `normal`

  这些项按默认位置填充，就像没有设置对齐内容值一样。

* `space-between`

  所有行在容器中平均分布。相邻两行间距相等。容器的垂直轴起点边和终点边分别与第一行和最后一行的边对齐。

* `space-around`

  所有行在容器中平均分布，相邻两行间距相等。容器的垂直轴起点边和终点边分别与第一行和最后一行的距离是相邻两行间距的一半。

* `space-evenly`

  所有行沿垂直轴均匀分布在对齐容器内。每对相邻的项之间的间距，主开始边和第一项，以及主结束边和最后一项，都是完全相同的。

* `stretch`

  拉伸所有行来填满剩余空间。剩余空间平均地分配给每一行。

* `safe`

  与对齐关键字一起使用。如果所选的关键字意味着项溢出对齐容器（data loss），则将采用备用策略对项进行对齐，就像启动了 `start` 对齐模式一样。

* `unsafe`

  与对齐关键字一起使用。无论元素和对齐容器的相对大小如何、是否会导致一些元素溢出可见范围（data loss），都使用给定的对齐值

7. 单个元素交叉轴的控制：`align-self`

- `auto`

  设置为父元素的 [`align-items`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-items) 值。	

- `normal`

- `self-start`

  Aligns the items to be flush with the edge of the alignment container corresponding to the item's start side in the cross axis.

- `self-end`

  Aligns the items to be flush with the edge of the alignment container corresponding to the item's end side in the cross axis.

- `flex-start`

  flex 元素会对齐到 cross-axis 的首端。

- `flex-end`

  flex 元素会对齐到 cross-axis 的尾端。

- `center`

  flex 元素会对齐到 cross-axis 的中间，如果该元素的 cross-size 尺寸大于 flex 容器，将在两个方向均等溢出。

8. 剩余空间分配: `flex-grow` 0（不分配剩余空间）

9. 空间不足缩小: `flex-shrink` 0(不缩小)

10. 基准尺寸：`flex-basic` 当主轴为横轴`flex-basic`则表示宽 当主轴为纵轴`flex-basic`则表示高  优先级`min` / `max` > `flex-basic` > `width` / `height`

11. 组合属性: `flex : flex-grow flex-shrink flex-basic` 
12. 控制flex元素的排列顺序: `order` 数字越大越靠后(可任意整数)

13. `position:absolute` 会对flex布局有影响，相反 `position: relative`没有 `float`同样不会影响

### 16. getComputedStyle

`Window.getComputedStyle()`方法返回一个对象，该对象在应用活动样式表并解析这些值可能包含的任何基本计算后报告`元素的所有CSS属性的值`。 私有的CSS属性值可以通过对象提供的API或通过简单地使用CSS属性名称进行索引来访问。



### 17.margin中的auto

​	`自动占用可用空间`: margin: auto(左右margin自动铺满) margin-right: auto (右侧margin自动铺满)



### 18.骰子三个点

利于flex

```html
<style>
      ul {
        width: 300px;
        height: 300px;
        background-color: gray;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        margin: 0;
        padding: 0;
      }
      li {
        list-style: none;
        width: 50px;
        height: 50px;
        background-color: yellow;
        border-radius: 50%;
      }
      li:first-child {
        align-self: flex-start;
      }
      li:nth-child(2) {
        align-self: center;
      }
      li:nth-child(3) {
        align-self: flex-end;
      }
    </style>
  </head>
  <body>
    <ul>
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </body>
```



### 19. 移动端布局方案

1. 流式布局(百分比)

   不改变字体大小，只改变元素尺寸（百分比）

2. 弹性盒模型(flex)

3. rem + 媒体查询/flexible.js（已被废弃）

4. rem + vw

   根元素字体大小设置 vw



### 20. DOM事件流

dom事件流分为三个阶段

1. 事件捕获阶段

   从最外层元素开始查找至事件触发元素，期间有绑定相同事件的元素，则触发该事件

2. 事件触发阶段

   触发事件的元素触发绑定事件

3. 事件冒泡阶段

   从触发事件元素开始，向外传递，直至根元素结束，碰到相同事件，则触发

`阻止冒泡`: e.stopPropagation()

`event.target` 和 `this`:  event.target: 触发元素   this: 绑定事件元素	



### 21. 事件委托

由于事件冒泡将子元素事件代理到父元素，提高性能

```html
<style>
      ul {
        padding: 0;
        margin: 0;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
      }
      li {
        list-style: none;
        width: 100px;
        height: 100px;
        background-color: rebeccapurple;
        border-radius: 14px;
        cursor: pointer;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 30px;
        font-weight: bold;
        color: #fff;
      }
    </style>
  </head>
  <body>
    <ul class="father">
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
      <li>5</li>
      <li>6</li>
    </ul>
    <script>
      document.addEventListener("click", function (e) {
        //子元素事件代理到父元素事件
        if (e.target.tagName != "LI") return;
        e.target.remove();
      });
    </script>
  </body>
```

 

### 22. 阻止默认行为

` event.preventDefault();`



### 23.移动端点击问题 

`移动端点击事件延迟300ms是什么？`

​	*移动端浏览器会有一些默认的行为，比如双击缩放、双击滚动。*

​	*这些行为，尤其是双击缩放，主要是为桌面网站在移动端的浏览体验设计的。*

​	*而在用户对页面进行操作的时候，移动端浏览器会优先判断用户是否要触发默认的行为*

​	*用户在 iOS Safari 里边点击了一个链接。由于用户可以进行双击缩放或者双击滚动的操作，*

​	*当用户一次点击屏幕之后，浏览器并不能立刻判断用户是确实要打开这个链接，还是想要进行双击操作。*

​	*因此，iOS Safari 就等待 300 毫秒，以判断用户是否再次点击了屏幕。 鉴于iPhone的成功，*

​	*其他移动浏览器都复制了 iPhone Safari 浏览器的多数约定，包括双击缩放，*

​	*几乎现在所有的移动端浏览器都有这个功能*

`点击穿透`

​	*起因：事件执行的顺序是touchstart > touchend > click*

​        页面上有两个元素A和B。B元素在A元素之上。我们在B元素的touchstart事件上注册了一个回调函数，*

​        *该回调函数的作用是隐藏B元素。我们发现，当我们点击B元素，B元素被隐藏了，随后，A元素触发了click事件*

`解决`

​	*都可以用fastclick解决掉*

*原理:* 

   在检测到touchend事件的时候，会通过DOM自定义事件立即出发模拟一个click事件

   并把浏览器在300ms之后真正的click事件阻止掉

*使用方式： 在main.js中*

​         *安装，引入，改造全局的点击事件*

```javascript
if ('addEventListener' in document) {
            document.addEventListener('DOMContentLoaded', function () {
                FastClick.attach(document.body);
            }, false);
        }
```



### 24. Line-height 继承问题

line-height的继承规则如下：

- 如果具体的数字，就继承数字
- 如果写成2 或者 1.5，也是继承2或者1.5
- 如果写成百分比，则继承百分比**计算后**的值(根据元素字体大小)



### 25. css variable(CSS 变量)

#### `声明css变量`

`--` 声明css变量

```css
:root {
        --fontSize: 15px;
        --color: yellow;
        --width: 100px;
        --height: 100px;
      }
```

#### `使用css变量`

`var` 函数使用css变量

```javascript
.container {
        width: var(--width);
        height: var(--height);
        background-color: var(--color);
      }
```

`var` 传入第二个参数为默认值

```css
width: var(--width, 20);
```

声明时使用 `var`

```css
:root {
        --fontSize: 15px;
        --color: yellow;
        --width: 100px;
        --height: 100px;
        --bgColor: var(--color);
      }
```

`使用数值变量时，不能与单位连用，需使用calc`

```css
:root {
        --fontSize: 15px;
        --color: yellow;
        --width: 100px;
        --height: 100px;
        --bgColor: var(--color);
        --num: 100;
      }
      .container {
        width: calc(var(--num) * 1px);
        height: var(--height);
        background-color: var(--color);
      }
```



#### `css变量作用域名`

css声明的变量在声明标签内可被读取

与js作用规则一般

```css
:root {
        --rootColor: green;
      }
        div {
          --color: yellow;
        }
      span {
        --color: red;
        --spancolor: blue;
      }
      .container {
        width: 200px;
        height: 200px;
        background-color: var(--color);
      }
      span {
        color: var(--color);
        font-size: 30px;
        font-weight: bold;
      }
```



#### `js操作css变量`

```javascript
// 设置变量
document.body.style.setProperty('--primary', '#7F583F');

// 读取变量
document.body.style.getPropertyValue('--primary').trim();
// '#7F583F'

// 删除变量
document.body.style.removeProperty('--primary');
```



### 26. display: inline 的元素设置 margin 和 padding 会生效吗

nline 元素的 margin左右生效 上下不生效 ； padding 上下左右都生效



### 27. CSS选择器有哪些？哪些属性可以继承？

CSS选择符：id选择器(#myid)、类选择器(.myclassname)、标签选择器(p, h1, p)、相邻选择器(h1 + p)、子选择器（ul > li）、后代选择器（li a）、通配符选择器（*）、属性选择器（a[rel="external"]）、伪类选择器（a:hover, li:nth-child）

可继承的属性：font-size, font-family, color

不可继承的样式：border, padding, margin, width, height

优先级（就近原则）：!important > [ id > class > tag ]
!important 比内联优先级高



### 28.双冒号盒单冒号的区别

双冒号表示伪元素  单冒号表示伪类



### 29. css3新增的伪类

- `p:first-of-type` 选择属于其父元素的首个`<p>`元素的每个`<p>` 元素。
- `p:last-of-type` 选择属于其父元素的最后 `<p>` 元素的每个`<p>` 元素。
- `p:only-of-type` 选择属于其父元素唯一的 `<p>`元素的每个 `<p>` 元素。
- `p:only-child` 选择属于其父元素的唯一子元素的每个 `<p>` 元素。
- `p:nth-child(2)` 选择属于其父元素的第二个子元素的每个 `<p>` 元素。
- `:after` 在元素之前添加内容,也可以用来做清除浮动。
- `:before` 在元素之后添加内容。
- `:enabled` 已启用的表单元素。
- `:disabled` 已禁用的表单元素。
- `:checked` 单选框或复选框被选中。



### 30.. link和@import的区别

两者都是外部引用CSS的方式，它们的区别如下：

- link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。
- link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
- link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
- link支持使用Javascript控制DOM去改变样式；而@import不支持。



### 31.transition和animation的区别

- **transition是过度属性**，强调过度，它的实现需要触发一个事件（比如鼠标移动上去，焦点，点击等）才执行动画。它类似于flash的补间动画，设置一个开始关键帧，一个结束关键帧。
- **animation是动画属性**，它的实现不需要触发事件，设定好时间之后可以自己执行，且可以循环一个动画。它也类似于flash的补间动画，但是它可以设置多个关键帧（用@keyframe定义）完成动画



### 32.display:none与visibility:hidden的区别

### display:none与visibility:hidden的区别

这两个属性都是让元素隐藏，不可见。**两者区别如下：**

（1）**在渲染树中**

- `display:none`会让元素完全从渲染树中消失，渲染时不会占据任何空间；
- `visibility:hidden`不会让元素从渲染树中消失，渲染的元素还会占据相应的空间，只是内容不可见。

（2）**是否是继承属性**

- `display:none`是非继承属性，子孙节点会随着父节点从渲染树消失，通过修改子孙节点的属性也无法显示；
- `visibility:hidden`是继承属性，子孙节点消失是由于继承了`hidden`，通过设置`visibility:visible`可以让子孙节点显示；

（3）修改常规文档流中元素的 `display` 通常会造成文档的重排，但是修改`visibility`属性只会造成本元素的重绘；

（4）如果使用读屏器，设置为`display:none`的内容不会被读取，设置为`visibility:hidden`的内容会被读取。



### 33.**伪元素和伪类的区别和作用**

* 伪元素：**伪元素用于创建一些不在文档树中的元素,并为其添加样式**

* 伪类：伪类用于当已有元素处于的某个状态时,为其添加对应的样式,这个状态是根据用户行为而动态变化的。



### 34.css精灵图

`概念`：将多个小图片拼接到一个图片中。通过`background-position`和元素尺寸调节需要显示的背景图案

`优点`:

- 减少`HTTP`请求数，极大地提高页面加载速度
- 增加图片信息重复度，提高压缩比，减少图片大小
- 更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现

`缺点`:

- 图片合并麻烦
- 维护麻烦，修改一个图片可能需要从新布局整个图片，样式

### 35. css3有哪些新特性

- 新增选择器 `p:nth-child(n){color: rgba(255, 0, 0, 0.75)}`
- 弹性盒模型 `display: flex;`
- 多列布局 `column-count: 5;`
- 媒体查询 `@media (max-width: 480px) {.box: {column-count: 1;}}`
- 个性化字体 `@font-face{font-family: BorderWeb; src:url(BORDERW0.eot);}`
- 颜色透明度 `color: rgba(255, 0, 0, 0.75);`
- 圆角 `border-radius: 5px;`
- 渐变 `background:linear-gradient(red, green, blue);`
- 阴影 `box-shadow:3px 3px 3px rgba(0, 64, 128, 0.3);`
- 倒影 `box-reflect: below 2px;`
- 文字装饰 `text-stroke-color: red;`
- 文字溢出 `text-overflow:ellipsis;`
- 背景效果 `background-size: 100px 100px;`
- 边框效果 `border-image:url(bt_blue.png) 0 10;`
- 转换
  - 旋转 `transform: rotate(20deg);`
  - 倾斜 `transform: skew(150deg, -10deg);`
  - 位移 `transform: translate(20px, 20px);`
  - 缩放 `transform: scale(.5);`
- 平滑过渡 `transition: all .3s ease-in .1s;`
- 动画 `@keyframes anim-1 {50% {border-radius: 50%;}} animation: anim-1 1s;`



### 34.display: inline-bolock 空隙问题

`原因`: 原来**HTML代码中的`回车换行`被`转成`一个`空白符`，在字体不为0的情况下，空白符占据一定宽度**，所以inline-block的元素之间就出现了空隙

`解决`:

* 父元素设置font-size为0

* margin设置为负值。 `由于不同浏览器的间隙不同，所以该方法不建议`

* 父元素设置`display:table`或`word-spacing`为负值

  ```css
   body {
        letter-spacing: -1em;
      }
  ```

  

`拓展`:

* Word-sapcing

  标签或单词间的间距

* letter-sapcing

  用于文本间的间距



### 35.css3方面的优化

- `css`压缩与合并、`Gzip`压缩
- `css`文件放在`head`里、不要用`@import`
- 尽量用缩写、避免用滤镜、合理使用选择器



### 36. css content属性

css的`content`属性专门应用在 `before/after`伪元素上，用于来插入生成内容。最常见的应用是利用伪类清除浮动。



### 37. 重绘和回流（重排）是什么，如何避免？

- 重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘
- 回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流
- 注意：JS获取Layout属性值（如：`offsetLeft`、`scrollTop`、`getComputedStyle`等）也会引起回流。因为浏览器需要通过回流计算最新值
- 回流必将引起重绘，而重绘不一定会引起回流

**如何最小化重绘(repaint)和回流(reflow)**：

- 需要要对元素进行复杂的操作时，可以先隐藏(`display:"none"`)，操作完成后再显示
- 需要创建多个`DOM`节点时，使用`DocumentFragment`创建完后一次性的加入`document`
- 缓存`Layout`属性值，如：`var left = elem.offsetLeft;` 这样，多次使用 `left` 只产生一次回流
- 尽量避免用`table`布局（`table`元素一旦触发回流就会导致table里所有的其它元素回流）
- 避免使用`css`表达式(`expression`)，因为每次调用都会重新计算值（包括加载页面）
- 尽量使用 `css` 属性简写，如：用 `border` 代替 `border-width`, `border-style`, `border-color`
- 批量修改元素样式：`elem.className` 和 `elem.style.cssText` 代替 `elem.style.xxx`



### 38. CSS优化、提高性能的方法有哪些

- 多个`css`合并，尽量减少`HTTP`请求
- 将`css`文件放在页面最上面
- 移除空的`css`规则
- 避免使用`CSS`表达式
- 选择器优化嵌套，尽量避免层级过深
- 充分利用`css`继承属性，减少代码量
- 抽象提取公共样式，减少代码量
- 属性值为`0`时，不加单位
- 属性值为小于`1`的小数时，省略小数点前面的0
- `css`雪碧图



### 39. 你对 line-height 是如何理解的

- `line-height` 指一行字的高度，包含了字间距，实际上是下一行基线到上一行基线距离
- 如果一个标签没有定义 `height` 属性，那么其最终表现的高度是由 `line-height` 决定的
- 一个容器没有设置高度，那么撑开容器高度的是 `line-height` 而不是容器内的文字内容
- 把 `line-height` 值设置为 `height` 一样大小的值可以实现单行文字的垂直居中
- `line-height` 和 `height` 都能撑开一个高度，`height` 会触发 `haslayout`，而 `line-height`



### 40. li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法

> 行框的排列会受到中间空白（回车\空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占据空间，所以会有间隔，把字符大小设为0，就没有空格了
