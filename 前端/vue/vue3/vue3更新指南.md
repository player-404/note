# vue3更新指南
## 1.自定义指令的更新
### v-model 代替 sync
* vue2中，当我们想要双向更新子组件的 prop 时, 可以使用.sync修饰符

```ts
//父组件
<son :title.sync="title"/>

<script>
	export default {
		data() {
			return {
				title: ''
			}
		}
	}
</script>

//子组件
<template>
	<button @click="click">click</button>
</template>
<scipt>
export default {
	props: ['title'],
	emits: ['update:title'],
	methods: {
	click() {
		this.$emit('update:title', '改变数据')
		}
	}
}
<script>
```
* 在vue3中 使用 v-model 替代 sync
```ts
// 父组件
<script setup lang="ts">  
 import {ref} from 'vue'  
 import Son from './Son.vue'  
  
 const title = ref<string>('这是一条title数据');  
  
</script>  
<template>  
 <h1>父组件</h1>  
 <son v-model="title"/>  
 <h2>{{title}}</h2>  
</template>  
<style lang="scss" scoped></style>

//子组件
<script setup lang="ts">  
import {ref} from "vue";  
  
const props = defineProps({  
 "modelValue": {  
 type: String  
 }  
})  
const emit = defineEmits(['update:modelValue'])  
  
const value = ref<string>('')  
  
const click = (): void => {  
 emit('update:modelValue', '改变数据')  
}  
</script>  
<template>  
 <h1>子组件</h1>  
 <button @click="click">click</button>  
</template>  
<style lang="scss" scoped></style>
```

### 钩子函数的更新
- vue2钩子函数
	- bind
	- inserted
	- update
	- componentupdate
	- unbind
- vue3钩子函数
	- created
	- beforeMount
	- mounted
	- beforeUpdate
	- updated
	- beforeUnmounted
	- unmounted

另外，binding参数中，vue3增加了instance与dir参数，去除了 name 与 expression参数

