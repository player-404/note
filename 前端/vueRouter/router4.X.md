### 1.router-link & router-view


### 2.动态路由匹配
#### 2.1 带参数的动态路由匹配
**使用 `:` 传递动态参数**
```js
//router/index.js

const routes = [
{
	// 使用 : 传递动态参数
	path:'/home/:userid',
	name: 'home',
	component: () => (...)
}
]

```
现在`home/user` 或 `home/632546732`命中的都是用一个路由`/home`，只是传递的参数不同

**使用动态路由时，动态参数会在每个组件的`this.$route`**对像的`params`对象中暴露出来，属性就是`:`后的参数，值就是路由传递的值
```vue
	<script>
		export default {
			created() {
				const userId = this.$route.params.userid;
			}
		}
	</script>
```


下面是一些例子:

| 匹配模式                      | 匹配路径                 | $route.params           |
| ----------------------------- | ------------------------ | ---------------------- |
| /user/:username               | /user/zhangsan           | {username: 'zhangsan'} |
| /user/:username/posts/:postid | /user/zhangsan/posts/123 | {username: 'zhangsan', postid: 123}                       |


**query & params:**
- query: url的参数，？后面的字段： `https://baidu.com?name=zhangsan`
- params: 动态路由参数，`:`之后的字段

#### 2.2 路由参数的变化
**背景：**
使用动态路由参数时(`/user/:userId`)，如：`/user/123`和`/user/456`，只是参数不同，渲染的是同一个组件，**相同的组件实例会被重复使用，因此在第二个路由访问时不会出发生命周期**
**解决:**
使用`watch`监听或使用`导航守卫`
```js
export default {
  created() {
    console.log("user生命周期触发", this.params);
  },
  computed: {
    params() {
      return this.$route.params;
    },
  },
  //使用watch监听路由参数的变化
  watch: {
    params: {
      handler(newv) {
        console.log("change", newv);
      },
      deep: true,
    },
  },
};
</script>
```
#### 2.3 捕获404路由
**前提:**
- 我们可以在路由的路径中的`括号中加入正则表达式`，匹配任意路径
- 路由命中顺序按照配置的顺序来

捕获404路由：
```js
const routes = [
  {
    path: "/",
    name: "Home",
    component: Home,
  },
  {
    path: "/about",
    name: "About",
    // import 路由懒加载，组件会单独打包
    component: () => import(/* webpackChunkName: "about" */ "../views/About.vue"),
  },
  {
    //动态字段以 ':' 开始
    path: "/user/:userid",
    name: "User",
    component: () => import("../views/User.vue"),
  },
  // /后的所有路由都会命中，如/usd,/1223; 但该路由又在最后，只有上面的路由都没有命中的时候，才会命中该路由
  {
    path: "/(.*)",
    name: "notFound",
    component: () => import("../views/NotFound.vue"),
  },
];
```

#### 2.4 捕获所有路由
同样我们可以使用正则，捕获路由:
```js
const routes = [
	{
		//匹配`user路径下的所有路由`, 如：/user/api, /user/123
		path: '/user(.*)',
		name: 'user',
		component: () => import(...)
	},
	{
		// 匹配 movie/:参数 下的所有路由，如 /movie/123/sggdjh, 其中123为参数，不使用正则 则该路由无法被命中
		path: '/movie/:movieid(.*)'
	}
]
```

### 3.路由匹配语法
**我们可以在路由路径中使用正则，匹配路由**
#### 3.1 参数中的自定义正则
看以下两个路由：
```js
const routes = [
	{
		path: "/:id",
		name: 'router1',
		...
	},
	{
		path: "/:word",
		name: 'router2',
		...
	}
]
```
当我们访问 `/667576`或`/ashdjks`, 命中的都第一个路由，实际上只要我们访问
`/` + `变量` 的路径，命中的都是第一个路由 

**这时候，我们可以使用`正则`进行更加准确的匹配**
```js
const routes = [
	{
		// 匹配路由参数为数字的路径，前 \ 为转义字符
		path: "/:id(\\d+)",
		name: 'router1',
		...
	},
	{
		// 匹配路由参数为字母的路径
		path: "/:word([a-z|A-Z]+)",
		name: 'router2',
		...
	}
]
```
> 别忘了正则写在 `()` 中

#### 3.2 可重复的参数
使用  `*` 或 `+` 可将参数标记为重复
```js
const routes = [
	{
		// 匹配 /one, /one/two, /one/tow/three
		path: '/:id+',
		name: 'home',
		...
	},
	{
		// 匹配 /one, /one/two, /one/tow/three
		path: '/repeat/:id*',
		...
	}
]

```
> `注意`: 是让**参数**重复

结合自定义正则使用
```js
const routes = [
	{
		// 匹配重复数字参数: /1， /1/222，11/2/333
		path: '/:id(//d+)+',
		...
	}
]
```

#### 3.3 可选参数
使用 `*` 或 `?`代表可选参数：
- `*` 也代表重复参数，所以它可以代表该参数可`重复`或`省略`
- `?` 代表可选，可选参数不能重复
```js
const routes = [
	{
	  // 匹配 /repeat , /repeat/1, /repeat/1/1223
      path: "/repeat/:demo(\\d+)*",
      name: "repeat",
      component: () => import("../views/Repeat.vue"),
	},
	{
	  // 匹配 /repeat, /repeat/1
	  path: "/repeat/:demo(\\d+)?",
      name: "repeat",
      component: () => import("../views/Repeat.vue"),
    },
]
```


### 4 嵌套路由
命中的路由组件会显示在 `<router-view />`中，app中的 `<router-view />` 为顶层的，在组件中也可以有自己的 `<router-view />`, 并且可以一层一层嵌套下去
**嵌套路由**或者说**子路由**使用`children`字段，该字段数组

在home组件中添加`router-view`来渲染命中的子路由组件
```vue
//home.vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <h1>this is home components</h1>
    //router-view 用来显示匹配到的路由组件
    <router-view></router-view>
  </div>
</template>
```
在home路由中的`children`字段中添加子路由
```js
const routes = [
	{
		name: 'home',
		path: '/',
		component: Home,
		children: [
			{
				name: 'children',
				path: 'children',
				component: ...
			}
		]
	}
]
```
**如果直接渲染子路由组件，而不关心是否命中路由，将path设置为 “ ” ，即可**
```js
const routes = [
	{
		name: 'home',
		path: '/',
		component: Home,
		children: [
			//会直接渲染子路由组件
			{
				name: 'children',
				path: '',
				component: ...
			}
		]
	}
]
```
### 5. 编程式导航
**什么是编程式导航？**
使用 js Api 方式访问路由实例叫做编程式导航

#### 5.1 导航到其他位置

**使用：**
使用`$router.push`，等同于`<router-link to="" />`;
这个方法会向 history 栈添加一个新的记录，所以，`当用户点击浏览器后退按钮时，会回到之前的 URL`;

| 声明式               | 编程式 |
| -------------------- | ------ |
| < router-link to="..." > | this.$router.push(...)       |

**push传递字符串路径**
```vue
<script>
export default {
	methods: {
		click() {
			// 直接传递路径：{path: '/', name: 'home', ...}
			this.$router.push("/");
			// 带参数的路径：{path: '/:id', name: 'home'...}
			this.$router.push("/123");
		}
	}
}
</script>
```

**push传递路径对象**
```vue
<script>
export default {
	methods: {
		click() {
			// 传递带参数的路径：{path: '/:id', name:'home', ...}
			this.$router.push({path: '/123'})	
			
			// 或使用params: {path: '/:id', name:'home', ...}
			this.$router.push({name: 'home', params: {id: 123}})
			
			//带查询参数: {path: '/:id', name: 'home', ...}
			this.$router.push({ // => /123?name=zhangsan  
				path: '/123',
				query: {
					name: 'zhangsan'
				}
			})
			
			//带 hash， 结果是 /123#team
			this.$router.push({path: '/123', hash: '#team'})
		}
	}
}
</script>
```
> 1. **注意：path 与 params 不能同时使用，两者一起使用时, params会被忽略**
> 2. **router.push 返回 promise**

#### 5.2替换当前位置
**使用**
使用`router.replace`进行路由跳转，该方法不会向 history 添加新记录，`不可以在浏览器中回退到之前的URL`

| 声明式 | 编程式 |
| ------ | ------ |
| `<router-link :to="..." replace>`       |     `router.replace(...)`   |

**使用replace的几种方式:**
1.使用js：
```vue
<script>
export default {
  methods: {
    toUser() {
      this.$router
        .replace({
          name: "User",
          params: {
            userid: 1223123,
          },
          query: {
            name: "fuck",
          },
        })
        .then(() => {
          console.log("完成跳转");
        });
    },
  },
};
</script>
```

2.使用push, 添加replace属性
```vue
<script>
export default {
  methods: {
    toHome() {
      this.$router.push({ path: '/home', replace: true });
    },
};
</script>
```

3.使用router-link
```vue
<router-link to="/user/wejrgwe" replace>user</router-link>
```

#### 5.3 路由历史的前进与后退
使用`router.go`可以在`历史堆栈`中前进或后退指定的步数

```vue
<script>
export default {
  methods: {
    back() {
    // 后退上一个
      this.$router.go(-1);
    },
    goo() {
    // 前进前一个
      this.$router.go(1);
    },
  },
};
</script>
```
> 注意：使用replace无法前进或后退

### 6. 命名视图
**什么是命名视图**
为 `router-view` 命名，添加 `name` 属性，就是命名试图，如同具名插槽一般

**作用**
在我们想要在一个路由中展示多个`同级视图`的时候使用

**使用**
- router-view
添加name属性，未添加默认为 `default`
- routes
有多个视图，必然渲染多个组件，使用 `components` 属性

#### 6.1 展示同级的命名视图
**场景**
在跳转至某路由时，额外展示除默认外，其他指定的组件，这些组件与默认组件处于同一级别(兄弟组件)
```vue
<template>
	// 一般我们跳转指定路由时，都在默认视图中显示
	<router-view></router-view>

	// 下面时我们想跳转至 某路由时 额外显示的两个命名视图， 它们与默认视图为同一级别
    <router-view name="Slider"></router-view>
    <router-view name="TopNav"></router-view>
</template>
```
routes.js
```js
const routes = [
	{
		name: 'setting',
		path: '/setting',
		components: {
			deafult: () => import('../view/Setting.vue'),
			Slider: () => import('../componets/Slider.vue'),
			TopNav: () => import('../componets/TopNav.vue')
		}
	}
]
```

#### 6.2 嵌套的命名视图
**场景**
展示嵌套的同级命名视图

**使用**
在 `children` 选项中，添加 `components` 字段， 
在 子组件中，添加带 `name` 属性的 `router-view`

子路由组件：
```vue
<router-view></router-view>
<router-view name="Helper"></router-view>
    ```
routes:
```js
const routes = {
    path: "/setting",
    name: "setting",
    components: {
      default: () => import("../views/Setting.vue"),
      Slider: () => import("../components/Slider.vue"),
      TopNav: () => import("../components/TopNav.vue"),
    },
    children: [
      {
        path: "test",
        name: "test",
        components: {
          default: () => import("../views/Slider1.vue"),
          Helper: () => import("../views/Helper.vue"),
        },
      },
      {
        path: "test1",
        name: "test1",
        component: () => import("../views/Slider2.vue"),
      },
    ],
  }
  ```
