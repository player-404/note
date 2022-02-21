
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
	- mutations:
	- actions
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

> 一个 `store.dispatch` 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。


# module
由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。
为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块

## 局部state
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

