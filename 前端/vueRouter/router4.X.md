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
