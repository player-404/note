# vue2 笔记



### 1. v-html

输出`html` 

接收一个字符串或者变量

```vue
<div id="app" v-html="rawHtml"> //输出h1标签
    {{rawHtml}} //输出文本
  </div>
  
  export default {
  data: () => ({
    rawHtml: '<h1>this is a text</h1>'
  })
};
```



### 2.v-text

添加文本节点

### 3. 动态属性

`[]`表示动态属性

```vue
<div class="box" @[option]="click"></div> //等有@click

data: () => ({
        option: 'click'
    }),
    methods: {
        click() {
            console.log('click')
        }
    }
```



#### `计算属性默认带有getter方法， 需要的话可以手动添加setter方法`



### 4. class & style 动态绑定 

[自定义指令 — Vue.js (vuejs.org)](https://cn.vuejs.org/v2/guide/custom-directive.html#ad)

class可以接受一个数组列表

```vue
<div class="box" :class="[bg % 2 === 0 ? 'yellow' : 'blue', 'border']"></div>


```

也可以使用对象语法

```vue
<div :class="[{ active: isActive }, errorClass]"></div>
```



#### `style`也适用以上规则，且同样可以动态绑定

详见 [Class 与 Style 绑定 — Vue.js (vuejs.org)](https://cn.vuejs.org/v2/guide/class-and-style.html)



### 5. 条件渲染

当条件渲染想要绑定多个元素时使用`<template>`

```vue
<template v-if="shows">
        <h1>这里有多个元素</h1>
        <span>这里是span元素</span>
        <p>这里p元素</p>
      </template>
```

`使用key避免vue在条件渲染时复用组件，导致组件不更新`

#### `v-if vs v-show`

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。



### 6. 过滤器Filter

filter可以对指定的值进行修改，使用`|`表示，可以用在`{{}}`内与`v-bind`中

```html
<body>
    <div id="app">
        <span>{{name | customFilter}}</span> // name会作为参数传递给过滤器函数 customFilter
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script>
        new Vue({
            el: "#app",
            data: {
                name: 'zhangsan'
            },
            filters: {
                customFilter(value) {
                    if (!value) return;
                    let newValue = value.trim().toUpperCase();
                    return newValue
                }
            }
        })
    </script>
</body>
```

<img src="/Users/tom/Library/Application Support/typora-user-images/截屏2021-08-15 上午9.28.15.png" alt="截屏2021-08-15 上午9.28.15" style="zoom:50%;" />

### 7.v-pre

添加该指令的元素中的模版将不会被渲染 (加快编译)

```vue
<template>
  <div id="app">
      <span v-pre>{{text}}</span>
    </div>
  </div>
</template>
<script>
export default {
  data: () =>({
    text: '这里是一段文字'
  })
}
</script>
```

<img src="/Users/tom/Library/Application Support/typora-user-images/截屏2021-08-15 上午10.40.15.png" alt="截屏2021-08-15 上午10.40.15" style="zoom:50%;" />



### 8.路由懒加载

跳转到该路由时，再去加载路由，从而提高性能

```javascript
component: () => import(/* webpackChunkName: "about" */ '../views/About.vue') 
//import(...) 懒加载方式
```

