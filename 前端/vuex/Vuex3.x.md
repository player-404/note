
**vuex3 与 vue2配合使用**

# state 单一状态树
**单一状态树**： 每个应用将仅仅包含一个 store 实例

 ## 获取state的方式
### $state获取
可以使用`this.$store.state`获取
```vue
<script>
export deafult {
	computed: {
		count() {
			return this.$store.count
		}
	}
}
</script>
```
### 使用辅助函数获取
可以使用辅助函数`mapState`获取
- 使用对象方式获取
mapState可以接收一个对象
```vue
<script>
import {mapState} form 'vuex'
export deafult {
	computed: mapState({
		count: state => state.count
	})
}
</script>
```
- 使用数组方式获取
mapState可以接受一个数组
```vue
<script>
export default {
computed: {
mapState(['count'])
	}
}
</script>
```

### 将辅助函数属性与计算属性合并
在使用计算属性时，我们发现computed只能接受一个辅助函数，我们可以使用`...mapState`语法进行合并
```vue
<script>
export default {
computed: {
...mapState(['count'])
	}
}

```

> 使用 Vuex 并不意味着你需要将**所有的**状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。

# Getter
对 `store` 中的state中的属性进行计算，可以认为是`store`的计算属性

## 参数
每个 getters 接收两个参数:
- 第一个参数为state
```js
export default new Vuex.store({
state: {name: 'zahnnsgans'},
getters: {
getName(state) {
	return state.name
}
}
})
```
- getters 还可以接受其他 getter, 可以使用第二个参数 getters 获取其他getter
```js
export new Vuex.store({
state: {
	name: '张三'
},
getters: {
	getLength(state) {
			return state.name.length;
		},
	getNameAndLength(state, getters) {
			return state.name + getters.getLength;
		}
	},
```

### 使用工厂函数
getter 使用工厂函数可以让getter接收其他参数，但是需要以方法调用，会导致getter的缓存失效
```js
export default new Vuex.store({
state: {
	data: [
		{
			name: '张安'，
			age: 30
		},
		{
			name: '李四',
			age: 22
		}
	]
},
getters: {
		getData(state) {
			return function(name) {
				return state.data.find(item => item.name == '张三')
			}
		}
	}
})
```

# mutation
更改 Vuex 的 store 中的状态的`唯一方法是提交 mutation`

## 创建mutation
mutaion与普通方法类似，在mutations对象中直接注册即可
```js
cosnt store = {
	mutations: {
		//创建mutation
		change() {
		}
	}
}
```

## mutation参数
mutation接收两个参数
- 参数一 :state
第一个参数为state， 可以访问state并修改
```js
const store = {
	state: {
		name: 'zhangsan'
	}，
	mutations: {
	changeName(state) {
		state.name = '李四'
	}
	}
}
```
- 参数二: payload(可选)
该参数为用户传入，可以为任何数据类型，推荐为对象
```js
const store = {
	state: {
	name:'zhangsan'
	},
	mutations: {
	changeName(state, payload) {
		state,.name = payload.name;
	}
	}
}
```

## 调用mutation
### 普通调用方式
调用 mutation 使用 `store.commit([name], [payload])`
```vue
<script>
exporrt default {
	methods: {
	click() {
		this.$store.commit('changeName', {name: 'zahsnanm'})
		}
	}
}
</script>
```

### 对象调用方式
也可以使用对象的方式调用mutation
```vue
<script>
export default {
	methods: {
	click() {
		this.$store.commit({
			type: 'changeName',
			name: 'hshajs'
		})
	}
	}
}
</script>
```
当使用对象风格的提交方式，整个对象都作为载荷传给 mutation 函数

> 对于全局的同名 Mutation , 后面的 Mutation 方法 会覆盖之前的方法

### 辅助函数调用方式
与 `state` 与 `getters`不同的是，mutation 的辅助函数写在 `methods`中
```vue
<script>
import {mapMutation} from 'vuex';
export default {
	methods: {
		...mapMutation(['changeName'])
	}
}
</script>
```


## 使用常量代替mutation事件类型
使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式。这样可以使 linter 之类的工具发挥作用，同时把这些常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然：
```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
```
store:
```js
// store.js
import { createStore } from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = createStore({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // 修改 state
    }
  }
})
```
调用：
```vue
<script>
import { SOME_MUTATION } from './mutation-types'
import {mapMutation} from 'vuex';
export default {
	methods: {
		click() {
			this.$store.commit(SOME_MUTATION)
		}
	}
}
</script>
```
或使用辅助函数
```vue
<script>
import { SOME_MUTATION } from './mutation-types'
import {mapMutation} from 'vuex';
export default {
	methods: {
		...mapMutation(['SOME_MUTATION'])
	}
}
</script>
```


## mutation必须是同步函数
一条重要的原则就是要记住 **mutation 必须是同步函数**
 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的

 # action
 Action 类似于 mutation，不同在于：
-   Action `提交的是 mutation，而不是直接变更状态`。
-   Action `可以包含任意异步操作`。

注册一个action
```js
- const store = {
	state: {
		data: ''
	},
	mutatiom:{
		SET_DATA(state, payload) {
			state.data = payload.data;		
		}
	},
	//action用来提交mutaion, 可以包含任何异步操作
	actions: {
		setData(context) {
			context.commit('SET_DATA', {data: 'this is data'})
		}
	}
}
```

## action参数
- context
Action 函数接受一个与` store 实例`具有`相同方法`和`属性`的 context `对象`
包含：
	- state: 状态
	- getters:  计算属性
	- commit: 提交mutaion的方法
	- dispatch: 提交actions的方法
context 可以使用es6语法进行解构

- payload 载荷
Action函数接收的第二个参数为 payload由用户传入的车参数，推荐为对象
```js
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

## 分发dispatch
Action 通过 `store.dispatch`方法分发
```vue
<script>
	export default {
		methods: {
			click() {
				this.$store.displatch('increment', {name: '张三'})
			}
		}
	}
<script>
```

- 以载荷形式分发
```js
this.$store.dispatch('incrementAsync', {
  amount: 10
})
```

- 以对象形式分发
```js
this.$store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

> 使用action 进行异步操作时， 可以在其中提交 mutation 记录action的状态
```js
actions: {
  checkout ({ commit, state }, products) {
    // 把当前购物车的物品备份起来
    const savedCartItems = [...state.cart.added]
    // 发出结账请求，然后乐观地清空购物车
    commit(types.CHECKOUT_REQUEST)
    // 购物 API 接受一个成功回调和一个失败回调
    shop.buyProducts(
      products,
      // 成功操作
      () => commit(types.CHECKOUT_SUCCESS),
      // 失败操作
      () => commit(types.CHECKOUT_FAILURE, savedCartItems)
    )
  }
}

```

## 使用辅助函数(mapActions)分发Action
在组件中可以使用 `mapActions`辅助函数分发Action, `在methods中`
```vue
<script>
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
</script>
```

## 组合Action
 `store.dispatch` 可以处理被触发的 action 的处理函数返回的 Promise，并且 `store.dispatch` 仍旧返回 Promise：
```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```
dispatch 仍然返回 promise
```js
store.dispatch('actionA').then(() => {
  // ...
})
```

- action中可以返回另一个action
```js
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```

使用async / await ：
```js
// 假设 getData() 和 getOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

> - 一个 `store.dispatch` 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。
> - 对于全局的 同名Action, 后面的 Action 同名方法会覆盖之前的

# module
由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。
为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块

## 模块下的state
使用模块后，模块中的state将自动变为局部,最终`模块内的state将以对象属性的方式合并到全局的state中`
例如：在模块 test 的 state 中创建了 name 属性，在模块 test1 的 state 中创建了 name 属性，
合并后如下
```js
{
state: {
	// test模块下state
	test: {
		name: 'zhangsan'
	},
	//test1模块下的state
	test1:{
		name: 'lisi'
	}
  }
}
```
访问模块中的state：
- 普通方法访问
```vue
<script>
export default {
	created() {
		// 访问 test 模块下的 name 属性
		const name = this.$store.state.test.name;
	}
}
</script>
```
- 使用辅助函数
```vue
<script>
import {mapState} from 'vuex'
export default {
	computed: {
		...mapState(['test'])
	},
	created() {
		console.log(this.test.name);
	}
}
</script>
```

## 模块下的getters
模块中的getters将直接合并到全局的getters，`注意: getters中不允许同名的getter，含有同名的getter只会执行第一个，并会报错`

- 模块中的gatters接收三个参数
	- state  模块中的state
	- getters 合并后的 getter, 全局与局部的 getter 都能获取
	- rootState  全局的state, 合并后的全局state
```js
getters: {
	myGetter(state, getters, rootState) {
	...
	}
}
```

## 模块下的mutation
模块中的 Mutation 与 Getter 一致，都是`直接合并`到 `全局` 的`Mutation`中, 与 Getter 不同的是，`Mutation 允许同名`，触发同名的 Mutation 时，`都会触发`， 全局的 Mutation 中， 同名的 Mutation 方法，后面的 Mutation 覆盖之前的 Mutation 方法
```js
mutations: {
	// 同名的 mutation 都会触发
	Mua() {
	}，
	//允许同名
	Mua() {
	}
}
```

- 参数 （接收两个参数）
	- state 局部的state
	- payload 参数 由用户传入

## 模块下的Action
模块中的 Action 与 Mutation 规则一致， 都会直接合并到 全局 的 Action 中，并且允许同名的Action, 触发同名的 Action 时, `两个 Action 都会执行`，`全局的 Action中, 同名的 Action, 后面的Action方法会覆盖之前的方法`
```js
actions: {
	action() {},
	action() {
		// 两个都会执行
	}
}
```
- 参数
	- context
		-  rootState 全局的state(合并后)
		- state 局部 state
		- getters  全局的getters(合并后)
		- commit 分发mutation (模块中的commit 可以提交全局的)
		- dispatch 分发 action  (dispatch 可以提交全局的)
	- paylaod 用户传递的参数


> **模块于全局存在同名的mutaions或actions时，二者都会执行**

# module 的命名空间
## 使用命名空间
- **默认情况下，模块内部的 action 和 mutation 仍然是注册在 全局命名 空间的(getters 也是如此) ——这样使得多个模块能够对同一个 action 或 mutation 作出响应**
- 如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名(变成真正意义上的局部)

创建一个space命名空间
space.js
```js
export default {
  namespaced: true, // 开启命名空间
  state: {
    name: "sapce",
  },
  getters: {
    info(state, getters, rootState, rootGetters) {
      console.log("root getters", rootGetters);
      return `this is ${state.name} module`;
    },
  },  
  mutations: {
    change(state, payload) {
      console.log("namespace mutations");
      state.name = "changed name";
    },
  },
  actions: {
    changeAction({ rootState, rootGetters }, payload) {
      console.log("rootState", rootState);
      console.log("rootGetters", rootGetters);
      console.log("this is  namespaced payload");
    },
    subGobalMutation({ rootState, rootGetters, commit }, payload) {
      console.log("rootState", rootState);
      console.log("rootGetters", rootGetters);
      commit("same", { name: "这是模块传递的参数" }, { root: true });
    },
  },
};
```
index.js
```js
import Vue from "vue";
import Vuex from "vuex";

import space from "./modules/namespace";
Vue.use(Vuex);

export default new Vuex.Store({
  state: {},
  getters: {},
  mutations: {
    same(state, payload) {
      console.log("gobal mutation is running");
    },
  },
  actions: {},
  modules: {
    space, // 注册名为space的模块
  },
})
```

## 命名空间下模块的中的store属性调用
### state
访问state余未添加命名空间的模块一般, state会以对象的方式被合并到全局state，以模块名为对象名，模块state中的值为属性值
- 普通访问
```js
...
this.$store.state.space.name
```
- 辅助函数
```js
{
	...mapState(['space'])
}
...
{
	this.space.name;
}
```
- 传入命名空间的辅助函数
模块的空间名称字符串作为第一个参数传递给辅助函数
```js
computed: {
	...mapTSate('space', ['name'])
}
...
{
	this.name
}
```
### getters
#### 参数
getters 将会接收两个新的参数，可以接收四个参数
- state 局部state
- getters 局部getters
- rootState 全局state
- rootGetters 全局 getters
```js
export default {
  namespaced: true,
  getters: {
	info(state, getters, rootState, rootGetters) {}
  }
}
```

#### 使用局部getters
使用开启命名空间模块的getters时，以 `[模块名/getter]`的方式调用
调用space模块下的 info getter
- 普通调用方式
```js
...
{
	this.$store.getters['sapce/info']
}
...
```
- 辅助函数
```js
...
computed: {
	...mapGetters(['space/info'])
}
...
{
	this['space/info']
}
...
```
- 传入命名空间的辅助函数
模块的空间名称字符串作为第一个参数传递给辅助函数
```js
{
	...mapGetters("space", ["info"]),
}
...
{
	console.log(this.info);
}

```

### mutation
开启命名空间的模块 mutations 与 getters一样， 以 [moduleName/mutationName]方式调用
调用 space 模块下的mutation的 change 方法
- 普通方法调用
```js
{
	this.commit('space/change', {name: '这是用户传递的payload参数'})
}
```
- 使用辅助函数调用
```js
methods: {
	...mapMutations(['space/change'])
}
...
{
	this['space/change']()
}
```
- 传入命名空间的辅助函数
模块的空间名称字符串作为第一个参数传递给辅助函数
```js
{
...mapMutations("space", ["change"]),
}
...
{
	this.change();
}
```


#### 参数
参数与普通模块参数一致

### action
开启命名空间的模块 action 与 getters一样， 以 [moduleName/actionName]方式调用
调用sapce模块下的action中的changeAction方法
- 普通调用
```js
...
{
  this.$store.commit('space/changeAction')
}
...
```
- 辅助函数调用
```js
methods: {
	...mapActions(['space/changeAction'])
}
...
{
	this['space/changeAction']();
}
```
- 传入命名空间的辅助函数
模块的空间名称字符串作为第一个参数传递给辅助函数
```js
{
	...mapActions("space", ["changeAction"])
}
...
{
	this.changeAction();
}
```

#### 模块调用全局 mutaion/action
在开启命名空间的模块中的action中调用全局的mutation或action, 将 `{root: true}` 作为第三个参数传递给 mutation 或 action
```js
subGobalMutation({ rootState, rootGetters, commit }, payload) {
	//调用全局的mutation
    commit("same", { name: "这是模块传递的参数" }, { root: true });
	//调用全局的action
	dispatch('changeAction', {name: '这里时传递给全局action的参数'}, {root: true})
		
    },
```

### 参数
开启命名空间的模块的action额外接收 rootState与rootGetters为参数
- dispatch
- commit
- state,
- rootState,
- getters
- rootGetters

## createNamespacedHelpers
你可以通过使用 `createNamespacedHelpers` 创建基于某个命名空间辅助函数。它返回一个对象，对象里有新的绑定在给定命名空间值上的组件绑定辅助函数：
```js
mport { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
  computed: {
    // 在 `some/nested/module` 中查找
    ...mapState({
      a: state => state.a,
      b: state => state.b
    })
  },
  methods: {
    // 在 `some/nested/module` 中查找
    ...mapActions([
      'foo',
      'bar'
    ])
  }
}
```








