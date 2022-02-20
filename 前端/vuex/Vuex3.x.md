
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