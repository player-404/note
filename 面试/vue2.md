### 1. vue生命周期

* beforeCreate
* created
* beforeMounted
* mounted
* beforeUpdate
* updated
* beforeDestroy
* destroyed
* errorCaptured



### 2. 父子组件执行顺序

* 父 beforeCreate
* 父 created
* 父 beforeMount
* 子 beforeCreate
* 子 created
* 子 beforeMount
* 子 mounted
* 父 mounted



### 3. v-if 与 v-show 的区别

* v -if 根据渲染条件会讲元素销毁与重建，销毁的元素不会存在dom中，v-show控制的是元素css的display，元素始终存在dom中
* v-if 是惰性的，当渲染条件为假时，不会进行任何操作，只有条件改变为真时，才会进行渲染操作，v-show不管渲染条件如何都会渲染，并且只是进行简单的css切换
* v-if 切换开销更大，v-show 初始渲染开销更大 



1. vritual dom diff 算法

   1. 什么是vritual dom

      vritual dom: 虚拟dom , 指的是用JS模拟的DOM结构，换言之，vdom就是js对象

      ```html
      <ul id="list">
          <li class="item">Item1</li>
          <li class="item">Item2</li>
      </ul>
      ```

      

      ```javascript
      //映射成虚拟dom
      {
          tag: "ul",
          attrs: {
              id:&emsp;"list"
          },
          children: [
              {
                  tag: "li",
                  attrs: { className: "item" },
                  children: ["Item1"]
              }, {
                  tag: "li",
                  attrs: { className: "item" },
                  children: ["Item2"]
              }
          ]
      } 
      
      ```

   2. diff 算法

   

2. v-for 中 key 的作用

   key的作用主要是为了高效的更新虚拟DOM，使用key来给每个节点做一个唯一标识，Diff算法就可以正确的识别此节点，找到正确的位置区插入新的节点。



### 4. 页面组件加载会执行哪些声明周期

* beforeCreate

* created

  可以访问data, 不能访问dom

* beforeMounte

* mounted

  可以访问dom与data



### 5. 生命周期的使用

created => 请求数据

mounted => dom操作



### 6. ref

获取dom元素



### 7.组件传值

Props/emit. Provide/inject  eventBus



### 8. 动态路由

路由路径之后添加`:参数`， 该参数会添加到route.params中

```javascript
{
  path:'/about/:id',
  name:'about',
  component: () => import('...')
}


//组件中直接使用this.$route.params.id获取
```



### 9. Keep-alive

使用keep-alive会缓存组件

此时会多出两个生命周期 `actived`  `deactived`

添加了keep-alive的执行顺序：beforeCreate => created => beforeMounte => mounted => actived



### 10.nextTick

dom加载完毕之后执行其中的回调函数



### 11. vue中data为什么是一个函数

`data` 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 `data` 仍然是一个纯粹的对象，则所有的实例将**共享引用**同一个数据对象！通过提供 `data` 函数，每次创建一个新实例后，我们能够调用 `data` 函数，从而返回初始数据的一个全新副本数据对象。

- 根实例对象`data`可以是对象也可以是函数（根实例是单例），不会产生数据污染情况
- 组件实例对象`data`必须为函数，目的是为了防止多个组件实例对象之间共用一个`data`，产生数据污染。采用函数的形式，`initData`时会将其作为工厂函数都会返回全新`data`对象

### 12.MVVM是什么？和MVC有和区别

MVC

- Model(模型)：负责从数据库中取数据

- View(视图)：负责展示数据的地方

- Controller(控制器)：用户交互的地方，例如点击事件等等

- 思想：Controller将Model的数据展示在View上

  ![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4636ebbfa25049179c27a6b5ab8bb308~tplv-k3u1fbpfcp-watermark.awebp)



MVVM

- VM：也就是View-Model，做了两件事达到了数据的双向绑定 一是将【模型】转化成【视图】，即将后端传递的数据转化成所看到的页面。实现的方式是：数据绑定。二是将【视图】转化成【模型】，即将所看到的页面转化成后端的数据。实现的方式是：DOM 事件监听。
- 思想：实现了 View 和 Model 的自动同步，也就是当 Model 的属性改变时，我们不用再自己手动操作 Dom 元素，来改变 View 的显示，而是改变属性后该属性对应 View 层显示会自动改变（对应Vue数据驱动的思想）

**![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aac31b27392b4b0e90ca2f67c64c59c2~tplv-k3u1fbpfcp-watermark.awebp)**

区别

整体看来，MVVM 比 MVC 精简很多，不仅简化了业务与界面的依赖，还解决了数据频繁更新的问题，不用再用选择器操作 DOM 元素。因为在 MVVM 中，View 不知道 Model 的存在，Model 和 ViewModel 也观察不到 View，这种低耦合模式提高代码的可重用性

### 13. 使用过哪些Vue修饰符呢

#### 1.lazy

`lazy`修饰符作用是，改变输入框的值时value不会改变，当光标离开输入框时，`v-model`绑定的值value才会改变

```js
<input type="text" v-model.lazy="value">
<div>{{value}}</div>

data() {
        return {
            value: '222'
        }
    }
复制代码
```

![lazy1.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/746b221956384678812cd585e4fa7834~tplv-k3u1fbpfcp-watermark.awebp)

#### 2.trim

`trim`修饰符的作用类似于JavaScript中的`trim()`方法，作用是把`v-model`绑定的值的首尾空格给过滤掉。

```js
<input type="text" v-model.trim="value">
<div>{{value}}</div>

data() {
        return {
            value: '222'
        }
    }
复制代码
```

![number.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7416025f033140688ed68135f77c4b48~tplv-k3u1fbpfcp-watermark.awebp)

#### 3.number

`number`修饰符的作用是将值转成数字，但是先输入字符串和先输入数字，是两种情况

```js
<input type="text" v-model.number="value">
<div>{{value}}</div>

data() {
        return {
            value: '222'
        }
    }
复制代码
```

> 先输入数字的话，只取前面数字部分

![trim.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/322016b98d2c49fe87870c220814b266~tplv-k3u1fbpfcp-watermark.awebp)

> 先输入字母的话，`number`修饰符无效

![number2.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c3ae0a87592447df9938a446dd727e0a~tplv-k3u1fbpfcp-watermark.awebp)

#### 4.stop

`stop`修饰符的作用是阻止冒泡

```js
<div @click="clickEvent(2)" style="width:300px;height:100px;background:red">
    <button @click.stop="clickEvent(1)">点击</button>
</div>

methods: {
        clickEvent(num) {
            不加 stop 点击按钮输出 1 2
            加了 stop 点击按钮输出 1
            console.log(num)
        }
    }
复制代码
```

#### 5.capture

事件默认是由里往外`冒泡`，`capture`修饰符的作用是反过来，由外网内`捕获`

```js
<div @click.capture="clickEvent(2)" style="width:300px;height:100px;background:red">
    <button @click="clickEvent(1)">点击</button>
</div>

methods: {
        clickEvent(num) {
            不加 capture 点击按钮输出 1 2
            加了 capture 点击按钮输出 2 1
            console.log(num)
        }
    }
复制代码
```

#### 6.self

`self`修饰符作用是，只有点击事件绑定的本身才会触发事件

```js
<div @click.self="clickEvent(2)" style="width:300px;height:100px;background:red">
    <button @click="clickEvent(1)">点击</button>
</div>

methods: {
        clickEvent(num) {
            不加 self 点击按钮输出 1 2
            加了 self 点击按钮输出 1 点击div才会输出 2
            console.log(num)
        }
    }
复制代码
```

#### 7.once

`once`修饰符的作用是，事件只执行一次

```js
<div @click.once="clickEvent(2)" style="width:300px;height:100px;background:red">
    <button @click="clickEvent(1)">点击</button>
</div>

methods: {
        clickEvent(num) {
            不加 once 多次点击按钮输出 1
            加了 once 多次点击按钮只会输出一次 1 
            console.log(num)
        }
    }
复制代码
```

#### 8.prevent

`prevent`修饰符的作用是阻止默认事件（例如a标签的跳转）

```js
<a href="#" @click.prevent="clickEvent(1)">点我</a>

methods: {
        clickEvent(num) {
            不加 prevent 点击a标签 先跳转然后输出 1
            加了 prevent 点击a标签 不会跳转只会输出 1
            console.log(num)
        }
    }
复制代码
```

#### 9.native

`native`修饰符是加在自定义组件的事件上，保证事件能执行

```js
执行不了
<My-component @click="shout(3)"></My-component>

可以执行
<My-component @click.native="shout(3)"></My-component>
复制代码
```

#### 10.left，right，middle

这三个修饰符是鼠标的左中右按键触发的事件

```js
<button @click.middle="clickEvent(1)"  @click.left="clickEvent(2)"  @click.right="clickEvent(3)">点我</button>

methods: {
        点击中键输出1
        点击左键输出2
        点击右键输出3
        clickEvent(num) {
            console.log(num)
        }
    }
复制代码
```

#### 11.passive

当我们在监听元素滚动事件的时候，会一直触发onscroll事件，在pc端是没啥问题的，但是在移动端，会让我们的网页变卡，因此我们使用这个修饰符的时候，相当于给onscroll事件整了一个.lazy修饰符

```js
<div @scroll.passive="onScroll">...</div>
复制代码
```

#### 12.camel

```js
不加camel viewBox会被识别成viewbox
<svg :viewBox="viewBox"></svg>

加了canmel viewBox才会被识别成viewBox
<svg :viewBox.camel="viewBox"></svg>
复制代码
```

#### 12.sync

当`父组件`传值进`子组件`，子组件想要改变这个值时，可以这么做

```js
父组件里
<children :foo="bar" @update:foo="val => bar = val"></children>

子组件里
this.$emit('update:foo', newValue)
复制代码
```

`sync`修饰符的作用就是，可以简写：

```js
父组件里
<children :foo.sync="bar"></children>

子组件里
this.$emit('update:foo', newValue)
复制代码
```

#### 13.keyCode

当我们这么写事件的时候，无论按什么按钮都会触发事件

```js
<input type="text" @keyup="shout(4)">
复制代码
```

那么想要限制成某个按键触发怎么办？这时候`keyCode`修饰符就派上用场了

```js
<input type="text" @keyup.keyCode="shout(4)">
复制代码
```

Vue提供的keyCode：

```js
//普通键
.enter 
.tab
.delete //(捕获“删除”和“退格”键)
.space
.esc
.up
.down
.left
.right
//系统修饰键
.ctrl
.alt
.meta
.shift
复制代码
```

例如（具体的键码请看[键码对应表](https://link.juejin.cn?target=https%3A%2F%2Fzhidao.baidu.com%2Fquestion%2F266291349.html)）

```js
按 ctrl 才会触发
<input type="text" @keyup.ctrl="shout(4)">

也可以鼠标事件+按键
<input type="text" @mousedown.ctrl.="shout(4)">

可以多按键触发 例如 ctrl + 67
<input type="text" @keyup.ctrl.67="shout(4)">
```



### 14.Vue的理解

#### 一、什么是spa

单页面应用，所有必要的代码都通过单页面加载，根据用户的操作动态加载资源，而不会导致页面重新加载

#### 二、MPA和SPA的区别

多页应用MPA，每个页面都是一个主页面，都是独立的当我们在访问另一个页面的时候，都需要重新加载`html`、`css`、`js`文件

#### [#](https://vue3js.cn/interview/vue/spa.html#单页应用与多页应用的区别)单页应用与多页应用的区别

|                 | 单页面应用（SPA）         | 多页面应用（MPA）                   |
| :-------------- | :------------------------ | :---------------------------------- |
| 组成            | 一个主页面和多个页面片段  | 多个主页面                          |
| 刷新方式        | 局部刷新                  | 整页刷新                            |
| url模式         | 哈希模式                  | 历史模式                            |
| SEO搜索引擎优化 | 难实现，可使用SSR方式改善 | 容易实现                            |
| 数据传递        | 容易                      | 通过url、cookie、localStorage等传递 |
| 页面切换        | 速度快，用户体验良好      | 切换加载资源，速度慢，用户体验差    |
| 维护成本        | 相对容易                  | 相对复杂                            |

#### 单页应用优缺点

优点：

- 具有桌面应用的即时性、网站的可移植性和可访问性
- 用户体验好、快，内容的改变不需要重新加载整个页面
- 良好的前后端分离，分工更明确

缺点：

- 不利于搜索引擎的抓取

- 首次渲染速度相对较慢

  



### 15.v-if与v-show的区别

手段：v-if是动态的向DOM树内添加或者删除DOM元素；v-show是通过设置DOM元素的display样式属性控制显隐

编译过程：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换

编译条件：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译（编译被缓存？编译被缓存后，然后再切换的时候进行局部卸载); v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留

性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗

使用场景：v-if适合运营条件不大可能改变；v-show适合频繁切换

相同点 v-show 都可以动态控制着dom元素的显示隐藏

不同点：v-if 的显示隐藏是将DOM元素整个添加或删除，v-show 的显示隐藏是为DOM元素添加css的样式display，设置none或者是block，DOM元素是还存在的

在渲染多个元素的时候，可以把一个 元素作为包装元素，并使用v-if 进行条件判断，最终的渲染不会包含这个元素，v-show是不支持 语法



### 16.Vue挂在实例的过程中发生了什么

- `new Vue`的时候调用会调用`_init`方法
  - 定义 `$set`、`$get` 、`$delete`、`$watch` 等方法
  - 定义 `$on`、`$off`、`$emit`、`$off`等事件
  - 定义 `_update`、`$forceUpdate`、`$destroy`生命周期
- 调用`$mount`进行页面的挂载
- 挂载的时候主要是通过`mountComponent`方法
- 定义`updateComponent`更新函数
- 执行`render`生成虚拟`DOM`
- `_update`将虚拟`DOM`生成真实`DOM`结构，并且渲染到页面中



### 17.Vue生命周期

Vue生命周期总共可以分为8个阶段：创建前后, 载入前后,更新前后,销毁前销毁后，以及一些特殊场景的生命周期

| 生命周期      | 描述                               |
| :------------ | :--------------------------------- |
| beforeCreate  | 组件实例被创建之初                 |
| created       | 组件实例已经完全创建               |
| beforeMount   | 组件挂载之前                       |
| mounted       | 组件挂载到实例上去之后             |
| beforeUpdate  | 组件数据发生变化，更新之前         |
| updated       | 组件数据更新之后                   |
| beforeDestroy | 组件实例销毁之前                   |
| destroyed     | 组件实例销毁之后                   |
| activated     | keep-alive 缓存的组件激活时        |
| deactivated   | keep-alive 缓存的组件停用时调用    |
| errorCaptured | 捕获一个来自子孙组件的错误时被调用 |

使用场景分析

| 生命周期      | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| beforeCreate  | 执行时组件实例还未创建，通常用于插件开发中执行一些初始化任务 |
| created       | 组件初始化完毕，各种数据可以使用，常用于异步数据获取         |
| beforeMount   | 未执行渲染、更新，dom未创建                                  |
| mounted       | 初始化结束，dom已创建，可用于获取访问数据和dom元素           |
| beforeUpdate  | 更新前，可用于获取更新前各种状态                             |
| updated       | 更新后，所有状态已是最新                                     |
| beforeDestroy | 销毁前，可用于一些定时器或订阅的取消                         |
| destroyed     | 组件已销毁，作用同上                                         |

#### 请求放在created与mounted的区别

`created`是在组件实例一旦创建完成的时候立刻调用，这时候页面`dom`节点并未生成`mounted`是在页面`dom`节点渲染完毕之后就立刻执行的触发时机上`created`是比`mounted`要更早的两者相同点：都能拿到实例对象的属性和方法讨论这个问题本质就是触发的时机，放在`mounted`请求有可能导致页面闪动（页面`dom`结构已经生成），但如果在页面加载前完成则不会出现此情况建议：放在`create`生命周期当中



### 18.Vue中的v-if和v-for为什么不建议一起使用

`v-for`优先级比`v-if`高

`v-if` 和 `v-for` 同时用在同一个元素上，带来性能方面的浪费（每次渲染都会先循环再进行条件判断）

1. 如果避免出现这种情况，则在外层嵌套`template`（页面渲染不生成`dom`节点），在这一层进行v-if判断，然后在内部进行v-for循环

```vue
<template v-if="isShow">
    <p v-for="item in items">
</template>
```

2. 如果条件出现在循环内部，可通过计算属性`computed`提前过滤掉那些不需要显示的项

```vue
computed: {
    items: function() {
      return this.list.filter(function (item) {
        return item.isShow
      })
    }
}
```



### 19.动态给vue的data添加一个新的属性时会发生什么？怎样解决？

**现象：**在给已经设置好的data属性，添加新的数据时，不会反应到dom上

```vue
<template>
  <div class="about">
    <div v-for="(key, index) in obj" :key="index">{{key}}</div>
    <button v-on:click="click">add obj</button>
  </div>
</template>
<script>
export default {
  data: () => ({
    obj: {
      name: '张三',
      age: 78
    }
  }),
  methods: {
    click() {
      this.obj.des = "这里是加的obj的描述";
      console.log(this.obj)
    }
  }
}
</script>
```

点击按钮之后新添加的des属性并不会反应到dom上，但实际上obj已经有了des属性

<img src="/Users/tom/Library/Application Support/typora-user-images/截屏2021-08-17 下午9.20.59.png" alt="截屏2021-08-17 下午9.20.59" style="zoom:50%;"/>

**原因：** Vue劫持data中的属性添加get与set，但之后新增加的属性不会被劫持，故不会触发set，也就不会通知dep，wtacher也就不会去更新dom，但实际新增加的属性已经增加到data数据中了

原因是一开始`obj`的`foo`属性被设成了响应式数据，而`bar`是后面新增的属性，并没有通过`Object.defineProperty`设置成响应式数据

#### 解决方案

`Vue` 不允许在已经创建的实例上动态添加新的响应式属性

若想实现数据与视图同步更新，可采取下面三种解决方案：

- Vue.set()
- Object.assign()
- $forcecUpdated()

### Vue.set()

Vue.set( target, propertyName/index, value )

参数

- `{Object | Array} target`
- `{string | number} propertyName/index`
- `{any} value`

返回值：设置的值

通过`Vue.set`向响应式对象中添加一个`property`，并确保这个新 `property`同样是响应式的，且触发视图更新

### Object.assign()

直接使用`Object.assign()`添加到对象的新属性不会触发更新

应创建一个新的对象，合并原对象和混入对象的属性

### $forceUpdate

如果你发现你自己需要在 `Vue`中做一次强制更新，99.9% 的情况，是你在某个地方做错了事

`$forceUpdate`迫使`Vue` 实例重新渲染

PS：仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。

### 小结

- 如果为对象添加少量的新属性，可以直接采用`Vue.set()`
- 如果需要为新对象添加大量的新属性，则通过`Object.assign()`创建新对象
- 如果你实在不知道怎么操作时，可采取`$forceUpdate()`进行强制刷新 (不建议)

PS：`vue3`是用过`proxy`实现数据响应式的，直接动态添加新属性仍可以实现数据响应式

## 
