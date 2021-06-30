### 自定义属性(directive)

​	[自定义指令 | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/guide/migration/custom-directives.html#_2-x-语法)

​	除了vue内置事件外(v-click...), vue还支持自定义属性

* 全局创建

  ```javascript
  // js
  Vue.directive('highlight', {
    bind(el, binding, vnode) {
      el.style.background = binding.value
    }
  })
  //vue
  <p v-highlight="'yellow'">高亮显示此文本亮黄色</p>
  ```

* 组件(局部创建)

  ```javascript
  //组件 
  directives: {
    focus: {
      // 指令的定义
      inserted: function (el) {
        el.focus()
      }
    }
  }
  ```

* 钩子函数
  * `bind`: 第一次绑定元素时调用（只钓用一次）
  * `insert`：被绑定元素插入到父节点时调用
  * `update`：所在组件Vnode更新时调用
  * `componentUpdated`：指令所在组件及子组件更新时调用
  * `unbind`： 解绑时调用

* 钩子函数参数
  * `el`：指令所绑定的元素，可以用来直接操作 DOM。
  * `binding`：一个对象，包含以下 property：
    - `name`：指令名，不包括 `v-` 前缀。
    - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
    - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
    - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
    - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
    - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
  * `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-接口) 来了解更多详情。
  * `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

### 动态组件(is)

[动态组件](https://cn.vuejs.org/v2/guide/components.html#动态组件)

动态切换组件



