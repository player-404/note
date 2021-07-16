### 动态组件(v-is)

[动态组件](https://v3.cn.vuejs.org/guide/migration/custom-elements-interop.html#v-is-用于-dom-内模板解析解决方案)

动态切换组件，值为字符串



vue3缓存组件语法改变了

```vue

      <router-view v-slot="{ Component }" v-if="$route.meta.keepAlive">
        <keep-alive>
          <component :is="Component"/>
        </keep-alive>
      </router-view>
```

