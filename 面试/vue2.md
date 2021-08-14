### 1. vue生命周期

* beforeCreate
* created
* beforeMounted
* mounted
* beforeUpdate
* updated
* beforeDestroy
* destroyed



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



### 5. 生命周期的使用ß

created => 请求数据

mounted => dom操作



### 6. ref

获取dom元素



### 7.组件传值

Props/emit. Provide/inject  eventBus



### 8. 动态路由

路由之后添加`:参数`， 该参数会添加到route.params中

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

