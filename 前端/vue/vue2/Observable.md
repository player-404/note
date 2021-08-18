## 一、Observable 是什么

`Observable` 翻译过来我们可以理解成**可观察的**

我们先来看一下其在`Vue`中的定义

> `Vue.observable`，让一个对象变成响应式数据。`Vue` 内部会用它来处理 `data` 函数返回的对象

返回的对象可以直接用于渲染函数和计算属性内，并且会在发生变更时触发相应的更新。也可以作为最小化的跨组件状态存储器

```js
Vue.observable({ count : 1})
```

其作用等同于

```js
new vue({ count : 1})
```

在 `Vue 2.x` 中，被传入的对象会直接被 `Vue.observable` 变更，它和被返回的对象是同一个对象

在 `Vue 3.x` 中，则会返回一个可响应的代理，而对源对象直接进行变更仍然是不可响应的

## 二、使用场景

在非父子组件通信时，可以使用通常的`bus`或者使用`vuex`，但是实现的功能不是太复杂，而使用上面两个又有点繁琐。这时，`observable`就是一个很好的选择

创建一个`js`文件

```js
// 引入vue
import Vue from 'vue
// 创建state对象，使用observable让state对象可响应
export let state = Vue.observable({
  name: '张三',
  'age': 38
})
// 创建对应的方法
export let mutations = {
  changeName(name) {
    state.name = name
  },
  setAge(age) {
    state.age = age
  }
}
```

在`.vue`文件中直接使用即可

```js
<template>
  <div>
    姓名：{{ name }}
    年龄：{{ age }}
    <button @click="changeName('李四')">改变姓名</button>
    <button @click="setAge(18)">改变年龄</button>
  </div>
</template>
import { state, mutations } from '@/store
export default {
  // 在计算属性中拿到值
  computed: {
    name() {
      return state.name
    },
    age() {
      return state.age
    }
  },
  // 调用mutations里面的方法，更新数据
  methods: {
    changeName: mutations.changeName,
    setAge: mutations.setAge
  }
}
```