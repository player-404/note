**Vuex4.x 的相关使用可以参照 Vuex3.x, 其API等使用是完全一致的**

# 设置state类型
## 设置全局的state类型
createdStore 接收一个泛型，可以设置全局的state类型
```js
import rootState from "./rootInterface";
import prd from "./modules/prd/prd";
export default createStore<rootState>({
  state: {
    name: "123",
  },
  mutations: {},
  actions: {},
  modules: {
    prd,
  },
});
```
rootInterface.js:
```js
export default interface rootState {
  name: string;
}
```

## 设置模块的state类型
可以使用 `Module<S,R>`接口定义模型的state类型，vuex的内置声明
泛型 S 代表模块的state类型
泛型 R 代表全局的state类型
```js
import prdModuleType from "./prdInterface";
import { Module } from "vuex";
import rootState from "@/store/rootInterface";

const prd: Module<prdModuleType, rootState> = {
  namespaced: true,
  state() {
    return {
      name: 345,
    };
  },
  mutations: {
    changeName(state, payload: any): void {
      state.name = 12138;
      console.log(state.name);
    },
  },
};

export default prd;
```
prdInterface.js
```js
export default interface prdModuleType {
  name: number;
}
```

# vue3中使用vuex
可以通过调用 useStore 函数，来在 setup 钩子函数中访问 store。这与在组件中使用选项式 API 访问 this.$store 是等效的。
```js
import { useStore } from 'vuex'

export default {
  setup () {
    const store = useStore()
  }
}
```

## 访问State与Getter
为了访问 state 和 getter，需要创建 `computed` 引用以保留响应性，这与在选项式 API 中创建计算属性等效。
```js
import { computed } from 'vue'
import { useStore } from 'vuex'

export default {
  setup () {
    const store = useStore()

    return {
      // 在 computed 函数中访问 state
      count: computed(() => store.state.count),

      // 在 computed 函数中访问 getter
      double: computed(() => store.getters.double)
    }
  }
}
```

## 访问Mutation与Action
要使用 mutation 和 action 时，只需要在 `setup` 钩子函数中调用 `commit` 和 `dispatch` 函数。
```js
import { useStore } from 'vuex'

export default {
  setup () {
    const store = useStore()

    return {
      // 使用 mutation
      increment: () => store.commit('increment'),

      // 使用 action
      asyncIncrement: () => store.dispatch('asyncIncrement')
    }
  }
}
```

