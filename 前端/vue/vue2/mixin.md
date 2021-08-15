# mixin

### 1.什么是mixin

> 混入(mixing)提供了一种非常灵活的方式，来分发Vue组件中的可复用的功能。一个混入对象可以包含任意组件选项

minix`本质就是一个对象`，`对象中可以包含vue中的任意选项`：data、生命周期、methods、watch、computed ...

用来将组件中重复的代码或功能抽取出来

### 2.创建一个mixin

将点击按钮 显示/隐藏的功能及代码抽取出来

```javascript
//mixin.js
export default {
    data() {
        return {
            show: false
        }
    },
    methods: {
        click() {
            this.show = !this.show;
        }
    }
}
```

全局引入mixin

```javascript
import Vue from "vue";
import App from "./App.vue";
import router from "./router";
import store from "./store";
import mixin from './mixin/mixin' //引入
Vue.config.productionTip = false;

Vue.mixin(mixin); //使用
new Vue({
  router,
  store,
  render: (h) => h(App),
}).$mount("#app");
```

便可以在任意组件使用

```vue
<template>
  <div id="app">
    <div class="tips" v-show="show"></div>
    <button @click="click">显示/隐藏</button>
  </div>
</template>

<style lang="less">
.tips {
  width: 400px;
  height: 400px;
  background-color: yellowgreen;
  border: 2px solid red;
}
</style>
```

可以看到 全局组件使用后 任意组件变都可以只用mixin中创建的变量或者方法，不必在组件中重新创建

`若组件中的变量或函数与mixin中的相同，组件中的变量或函数会覆盖mixin中的`



局部引入mixin

```vue
<template>
  <div id="app">
    <div class="tips" v-show="show"></div>
    <button @click="click">显示/隐藏</button>
  </div>
</template>

<script>
import mixin from './mixin/mixin' //在当前组件中引入
export default {
  mixins: [mixin] //局部使用
}
</script>
<style lang="less">
.tips {
  width: 400px;
  height: 400px;
  background-color: yellowgreen;
  border: 2px solid red;
}
</style>
```



