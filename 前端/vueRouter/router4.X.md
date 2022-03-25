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


### 7. 重定向
使用关键字`redirect`可以对路由进行重定向
重定向本身是不需要引入`component`，因为它是指访问一个路由时，转到指定的路由，而这个指定的路由时路由表中注册过的路由

redirect参数：
- path字符串或对象
```js
const routes = [
 {
    name: "redirect",
    path: "/redirect",
    //重定向至setting路由，redirect可以时路径字符串，也可以是对象
    redirect: {
      name: "setting",
    },
    //重定向的路由必须注册了
  {
    path: "/setting",
    name: "setting",  
    component: ...
    }
  ]
```
- 函数，接受一个to参数，该参数代表当前$route
```js
const routes = [
	{
    name: "fun",
    path: "/fun",
    redirect: (to) => {
      console.log(to);
      return { path: "/", query: { name: to.name } };
    },
  ]
```


### 8.别名
我们可以为path(路径)设置别名，之后我们就可以使用别名访问路由

**alias接收一个数组**

根路由使用别名：
```js
const routes = [
	{
		name: 'home',
		path: '/home/:id',
		component: ...,
		alias: ['/haha', '/heihei/:id']
	}
]
```
- 访问别名`/haha`，会跳转到路由home中，但不会携带参数，因为别名中并未有设置参数，只有访问别名`/heihei/:id`才能懈怠参数
- 根路由必须以`/`开头，以`path`的格式

子路由使用别名：
```js
const routes = [
	{
    name: "test2",
    path: "/test2",
    component: () => import("../views/Test2.vue"),
    children: [
      {
        name: "people",
        path: "/people",
        //设置别名
        alias: ["/t", "haha"],
        component: () => import("../views/People.vue"),
      },
    ],
  }
  ]
```
设置子路由可以携带`\`，也不可不携带
- 携带 `\`
如上面代码，可以直接使用别名`/t`, `$router.push('/t')`访问test2的子路由，可以直接省略父路由路径，而直接使用别名路径
- 不携带 `\`
不携带 `\`则要使用父路径加别名的完整路径才能访问，`$router.puah('/test2/haha')`


### 9.路由组件参数
我们可以向路由中传递动态参数，参数存储在`$route.params`对象中
**我们也可以使用 props 接收路由的动态参数***

**使用**
参数：
- 参数为boolean
在路由中添加props字段
```js
const routes = [
	{
		name: 'home',
		path: '/home/:id',
		props: true,
		component: ...
	}
]
```
在组件的`props`选项中接受参数，用法参见`props`选项
```vue
	<script>
		export default {
			// 参数名要一致
			props: ['id']
		}
	</script>
```

- 命名路由
在命名路由中，则需要将props字段设置为对象，在该对象中设置
```js
const routes = [
	{
		name: 'home',
		path: '/home',
		components: {
			default: () => import(...),
			propsCom: () => import(...)
		},
		// 在props对象中设置
		props: {
			default: true,
			//设置命名路由的props
			propsCom: true
		}
	}
]
```
- 参数为普通对象
当props为对象时，除了传递动态参数给组件，在props中定义的其他属性，会作为静态数据传递给组件
```js
const routes = [
	{
		name: 'home',
		path: '/home/:id',
		componet: ...,
		props: {
			id: 123,
			names: '张三'
		}
	}
]
```

```vue
<script>
	export default {
		props: ['id', 'names']
	}
</script>
```

- 函数模式
props也可以接收函数，`$route`作为参数传递给该函数
```js
const routes = [
	{
		name: 'home',
		path: '/home',
		//该函数返回一个对象
		props: (route) => ({query: route.query.id}) 
	}
]
```

### 10.历史记录模式
可以手动将vue-router 修改为 history 或 hash模式

#### 10.1 vue2中的修改
vue2 中，在 `new VueRouter`中设置 `mode`属性
```js
const router = new VueRouter({
	//将mode属性设置为 history 或 hash
	mode: 'hash'
})
```

#### 10.2 vue3中的修改
vue3中，在 createRouter中 设置 history属性
- hash: createWebHashHistory
- history: createWebHsitory
```js
import { createRouter, createWebHashHistory } from 'vue-router'

const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    //...
  ],
})
```

#### 10.3 history模式下生产环境的问题
由于我们的应用是一个单页的客户端应用，history模式下,  如果没有适当的服务器配置，用户在浏览器中直接访问 `https://example.com/user/id`，就会得到一个 404 错误

**解决**
在服务端进行设置

- nginx
``` json
location / {
  try_files $uri $uri/ /index.html;
}
```

- nodejs
```js
const http = require('http')
const fs = require('fs')
const httpPort = 80

http
  .createServer((req, res) => {
    fs.readFile('index.html', 'utf-8', (err, content) => {
      if (err) {
        console.log('We cannot open "index.html" file.')
      }

      res.writeHead(200, {
        'Content-Type': 'text/html; charset=utf-8',
      })

      res.end(content)
    })
  })
  .listen(httpPort, () => {
    console.log('Server listening on: http://localhost:%s', httpPort)
  })
```
- express + nodejs
对于 Node.js/Express，可以考虑使用 [connect-history-api-fallback 中间件](https://github.com/bripkens/connect-history-api-fallback)。


### 11. 路由守卫
**当一个导航触发时，全局前置守卫按照`创建顺序调用`。`守卫是异步解析执行`，此时导航在所有守卫 `resolve` 完之前一直处于 `等待中`。**

**守卫的回调函数为 pomise 函数**


#### 11.1 全局路由守卫
全局路由守卫写在全局

##### 11.1.1 全局前置守卫 beforeEach
在跳转路由之前触发，该守卫为全局，
**回调参数**
- to：即将进入的路由对象
- form：当前导航正要离开的路由
- next: 该方法用来 `resolve` 守卫钩子，接收 路由对象、boolean、Error对象、 path字符串
- return **vueRouter 4.x 独有**
	在vueRouter 中，可以在守卫回调中返回值，与next接受的参数一致，效果也一致
```js
router.beforeEach((to, from, next) => {
//不调用该方法，路由会一直处于等待的状态中，会执行下一个路由
	next();
})
```

##### 11.1.2 全局解析守卫 beforeResolve
在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用
```js
router.beforeResolve((to, from, next) => {
	next();
})
```


##### 11.1.3 全局后置守卫 afterEach
在进入路由之后触发， 它不接受next函数作为第三个参数，或可以返回值，所以不能更改路由

后置守卫一般用来对页面的分析，页面标题的更改等或其他更多功能

**在 vueRouter 4.x 中 afterEach 接受  `failure` 作为第三个参数，一般用作错误处理**

```js
router.afterEach((to, from, failure) => {
	// do something
})
```

#### 11.2路由独享守卫
路由独享守卫写在路由列表的指定路由中，参数与全局前置路由守卫一样
```js
const routes = [
	{
		name: 'Home',
		path: '/',
		component: () => import(...),
		beforeEnter: (to, from , next) => {
			//...do something
		}
	}
]
```
**在 vueRouter 4.x 中** 路由独享守卫还可以是一个数组

```js
const fun = () => ();
const fun1 = () => ();

const routes = [
	{
		name: 'Home',
		path: '/',
		component: () => import(...),
		beforeEnter: [fun, fun1]
	}
]
```

#### 11.3 组件内的路由守卫
组件内的守卫，就是在组件内部的守卫

#### 11.3.1 boforeRouteEnter
在该组件的路由被别确认前使用
参数：该路由守卫与全局前置路由守卫的参数一致
this: 由于是在路由确认前使用，因此，组件实例并未创建，`无法获取this`
```vue
//home 组件
<script>
export default {
	beforeRouteEnter(to, from, next) {
		console.log('组件路由守卫')
		next()
	}
}
</script>
```
> 该路由在 `全局前置守卫` 之后执行，在`全局解析守卫`之前执行

#### 11.3.2 beforeRouteUpdate
- 在当前路由改变，但是该组件被复用时调用

- 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候

- 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。

- 可以访问组件实例 `this`

**路由改变，但组件复用时，路由独享守卫，与 组件前置守卫不糊触发， 只会触发组件的路由更新守卫(beforeRouteUpdate)**

```vue
<script>
export default {
  beforeRouteEnter(to, from, next) {
    console.log("组件前置路由守卫被触发");
    console.log(this);
    next();
  },
  beforeRouteUpdate(to, form) {
    console.log("更新路由 触发了");
  },
};
</script>
```

#### 11.3.3 beforeRouteLeave
这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 `next(false)` 来取消。

该路由守卫在离开当前路由之前触发

```vue
<script>
export default {
  beforeRouteEnter(to, from, next) {
    console.log("组件前置路由守卫被触发");
    console.log(this);
    next();
  },
  beforeRouteUpdate(to, form) {
    console.log("更新路由 触发了");
  },
  beforeRouteLeave(to, from, next) {
    console.log("离开路由了");
    next();
  },
};
</script>
```

#### 11.3.4 完整路由导航解析流程
1.  导航被触发。
2.  在失活的组件里调用 `beforeRouteLeave` 守卫。3.  调用全局的 `beforeEach` 守卫。
4.  在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5.  在路由配置里调用 `beforeEnter`。
6.  解析异步路由组件。
7.  在被激活的组件里调用 `beforeRouteEnter`。
8.  调用全局的 `beforeResolve` 守卫 (2.5+)。
9.  导航被确认。
10.  调用全局的 `afterEach` 钩子。
11.  触发 DOM 更新。
12.  调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。
13. 

#### 11.4路由元信息 meta
可以在 `routes` 中配置meta, 它本质上是一个对象，可以在 `$route.meta`中访问到或 `路由守卫`中访问到
```js
const routes = [
  {
    path: '/posts',
    component: PostsLayout,
    children: [
      {
        path: 'new',
        component: PostsNew,
        // 只有经过身份验证的用户才能创建帖子
        meta: { requiresAuth: true }
      },
      {
        path: ':id',
        component: PostsDetail
        // 任何人都可以阅读文章
        meta: { requiresAuth: false }
      }
    ]
  }
]


```
在路由守卫中访问 `meta` 字段
```js
router.beforeEach((to, from) => {
  // 而不是去检查每条路由记录
  // to.matched.some(record => record.meta.requiresAuth)
  if (to.meta.requiresAuth && !auth.isLoggedIn()) {
    // 此路由需要授权，请检查是否已登录
    // 如果没有，则重定向到登录页面
    return {
      path: '/login',
      // 保存我们所在的位置，以便以后再来
      query: { redirect: to.fullPath },
    }
  }
})
```

### 12过渡动效
#### 12.1 vue-router4.x
想要在你的路径组件上使用转场，并对导航进行动画处理，你需要使用 [v-slot API](https://router.vuejs.org/zh/api/#router-view-s-v-slot)：
```vue
<router-view v-slot="{ Component }">
  <transition name="fade">
    <component :is="Component" />
  </transition>
</router-view>
```
v-slot接受一下参数：
-   `href`：解析后的 URL。将会作为一个 `<a>` 元素的 `href` 属性。如果什么都没提供，则它会包含 `base`。
-   `route`：解析后的规范化的地址。
-   `navigate`：触发导航的函数。 **会在必要时自动阻止事件**，和 `router-link` 一样。例如：`ctrl` 或者 `cmd` + 点击仍然会被 `navigate` 忽略。
-   `isActive`：如果需要应用 [active class](https://router.vuejs.org/zh/api/#active-class)，则为 `true`。允许应用一个任意的 class。
-   `isExactActive`：如果需要应用 [exact active class](https://router.vuejs.org/zh/api/#exact-active-class)，则为 `true`。允许应用一个任意的 class。

**单个路由的过渡**
上面的用法会对所有的路由使用相同的过渡。如果你想让每个路由的组件有不同的过渡，你可以将[元信息](https://router.vuejs.org/zh/guide/advanced/meta.html)和动态的 `name` 结合在一起，放在`<transition>` 上：
```js
const routes = [
  {
    path: '/custom-transition',
    component: PanelLeft,
    meta: { transition: 'slide-left' },
  },
  {
    path: '/other-transition',
    component: PanelRight,
    meta: { transition: 'slide-right' },
  },
]
```

```vue
<router-view v-slot="{ Component, route }">
  <!-- 使用任何自定义过渡和回退到 `fade` -->
  <transition :name="route.meta.transition || 'fade'">
    <component :is="Component" />
  </transition>
</router-view>
```

#### 12.2 vue-router 3.x 
直接使用 `transition` 标签包裹即可
```vue
<transition>
  <router-view></router-view>
</transition>
```


### 13.滚动行为
若要在跳转到新路由时，使页面保持原先的滚动位置，可以在router实例中，使用 `scrollBehavior` 方法
```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```
该函数可以返回一个 [`ScrollToOptions`](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions) 位置对象:
```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    // 始终滚动到顶部
    return { top: 0 }
  },
})
```
你也可以通过 `el` 传递一个 CSS 选择器或一个 DOM 元素。在这种情况下，`top` 和 `left` 将被视为该元素的相对偏移量。
```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    // 始终在元素 #main 上方滚动 10px
    return {
      // 也可以这么写
      // el: document.getElementById('main'),
      el: '#main',
      top: -10,
    }
  },
})
```
#### 13.1 延迟滚动
有时候，我们需要在页面中滚动之前稍作等待。例如，当处理过渡时，我们希望等待过渡结束后再滚动。要做到这一点，你可以返回一个 Promise，它可以返回所需的位置描述符。下面是一个例子，我们在滚动前等待 500ms：
```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({ left: 0, top: 0 })
      }, 500)
    })
  },
})
```

### 14.路由懒加载
路由的 component/components 接受一个返回promise的组件函数，vue-router 只会在第一次进入页面的时候才会获取这个函数，然后使用缓存数据，在打包时，会自定分割代码
```js
// 将 ter
// import UserDetails from './views/UserDetails'
// 替换成
const UserDetails = () => import('./views/UserDetails')

const router = createRouter({
  // ...
  routes: [{ path: '/users/:id', component: UserDetails }],
})
```
#### 14.1把组件安组分块
有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用[命名 chunk](https://webpack.js.org/guides/code-splitting/#dynamic-imports)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)：
```js
const UserDetails = () =>
  import(/* webpackChunkName: "group-user" */ './UserDetails.vue')
const UserDashboard = () =>
  import(/* webpackChunkName: "group-user" */ './UserDashboard.vue')
const UserProfileEdit = () =>
  import(/* webpackChunkName: "group-user" */ './UserProfileEdit.vue')
```
webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

### 15.动态路由
#### 15.1 添加路由 addRoute
可以使用`$router.addRoute` 向路由表添加一个路由
```js
router.addRoute({ path: '/about', component: About })
```

#### 15.2删除路由 removeRoute
删除路由的几种方式：
- `$router.removeRoute`
```js
//删除路由
router.removeRoute('about')
```

- 添加同名路由
```js
router.addRoute({ path: '/about', name: 'about', component: About })
// 这将会删除之前已经添加的路由，因为他们具有相同的名字且名字必须是唯一的
router.addRoute({ path: '/other', name: 'about', component: Other })
```

- 调用 `router.addRoute()` 返回的回调：
```js
const removeRoute = router.addRoute(routeRecord)
removeRoute() // 删除路由如果存在的话
```

#### 15.3查看现有路由
Vue Router 提供了两个功能来查看现有的路由：

- `router.hasRoute()：`检查路由是否存在。
 - `router.getRoutes()：`获取一个包含所有路由记录的数组。