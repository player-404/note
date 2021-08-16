# 渲染函数render

### 1.什么是渲染函数

Vue 推荐在绝大多数情况下使用模板来创建你的 HTML。然而在一些场景中，你真的需要 JavaScript 的完全编程的能力。这时你可以用**渲染函数**（render），它比模板更接近编译器。

```javascript
export default {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // 标签名称
      this.$slots.default // 子节点数组
    )
  },
}
```

**render是一个函数，它默认接收一个函数(createElemet)以创建html，这个函数返回的是虚拟DOM**

Vue 通过建立一个**虚拟 DOM** 来追踪自己要如何改变真实 DOM。请仔细看这行代码：

```javascript
return createElement('h1', this.blogTitle)
```

`createElement` 到底会返回什么呢？其实不是一个*实际的* DOM 元素。它更准确的名字可能是 `createNodeDescription`，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，包括及其子节点的描述信息。我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为“**VNode**”。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。

#### createElement 参数

该函数接收三个参数:

* **nodeName**（必填）  类型：String | object | function 

  html标签名、组件

* **attribute**（可选） 类型：object

  创建属性名，会添加到dom上

* **Vnodes**（可选） 类型：String ｜ Array

  子级虚拟节点, 由`createElement()`构成，也可直接只用字符串生成文本节点



**参数attribute:**

attribute可以为创建的Vnode添加多个属性

添加 **class**

```javascript
 render(h) {
        return h(
            `div`,
            {
                class: ['box']
            },
            '这里是渲染函数创建的html'
        )
    },
```

添加 **style**

```javascript
 render(h) {
        return h(
            `div`,
            {
                class: ['box'],
                style: {
                    color: 'red',
                    fontSize: '28px',
                    fontWeight: 'bold'
                },
            },
            '这里是渲染函数创建的html'
        )
    },
```

为创建的dom添加其他属性 **attrs**

```javascript
 render(h) {
        return h(
            `div`,
            {
                class: ['box'],
                style: {
                    color: 'red',
                    fontSize: '28px',
                    fontWeight: 'bold'
                },
                attrs: {
                    id: 123,
                    name: 'box'
                }
            },
            '这里是渲染函数创建的html'
        )
    },
```

添加 **props**

```javascript
 render(h) {
        return h(
            `div`,
            {
                class: ['box'],
                style: {
                    color: 'red',
                    fontSize: '28px',
                    fontWeight: 'bold'
                },
                attrs: {
                    id: 123,
                    name: 'box'
                },
                props: {
                    text: 'box'
                }
            },
            '这里是渲染函数创建的html'
        )
    },
```

添加事件 **on**

```javascript
 render(h) {
        return h(
            `div`,
            {
                class: ['box'],
                style: {
                    color: 'red',
                    fontSize: '28px',
                    fontWeight: 'bold'
                },
                attrs: {
                    id: 123,
                    name: 'box'
                },
                props: {
                    text: 'box'
                },
                domProps: {
                    innerHTML: '<h1>123</h1>',
                },
                on: {
                    click: this.clicks
                }
            },
            '这里是渲染函数创建的html'
        )
    },
```



#### render创建的虚拟dom必须是唯一的

组件树中的所有 VNode 必须是唯一的。这意味着，下面的渲染函数是不合法的：

```javascript
render: function (createElement) {
  var myParagraphVNode = createElement('p', 'hi')
  return createElement('div', [
    // 错误 - 重复的 VNode
    myParagraphVNode, myParagraphVNode
  ])
}
```

如果你真的需要重复很多次的元素/组件，你可以使用工厂函数来实现。例如，下面这渲染函数用完全合法的方式渲染了 20 个相同的段落：

```javascript
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 20 }).map(function () {
      return createElement('p', 'hi')
    })
  )
}
```

