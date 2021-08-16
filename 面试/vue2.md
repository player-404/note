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



### 11. vue中data为什么是一个对象

`data` 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 `data` 仍然是一个纯粹的对象，则所有的实例将**共享引用**同一个数据对象！通过提供 `data` 函数，每次创建一个新实例后，我们能够调用 `data` 函数，从而返回初始数据的一个全新副本数据对象。



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

