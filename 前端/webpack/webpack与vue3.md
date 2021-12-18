# 版本

#### vue不同版本区别

![截屏2021-12-15 下午3.07.30](https://raw.githubusercontent.com/player-404/images/main/%E6%88%AA%E5%B1%8F2021-12-15%20%E4%B8%8B%E5%8D%883.07.30.png)



#### 打包Vue

​	webpack打包vue3：

​		loader: `vue-loader@next`

​		插件： `@vue/compiler-sfc`、`VueLoaderPlugin`

​	

​	完整配置:

​	webpack.config.js

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { VueLoaderPlugin } = require("vue-loader/dist/index");
module.exports = {
  entry: "./main.js",
  output: {
    path: `${__dirname}/dist`,
    filename: "[name]-boundle.js",
    clean: true,
  },
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
      {
        test: /\.js$/i,
        loader: "babel-loader",
        exclude: /node_modules/,
      },
      {
        test: /\.vue$/i,
        loader: "vue-loader",
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./index.html",
      filename: "[name].html",
      title: "vue app",
      inject: "body",
    }),
    new VueLoaderPlugin(),
  ],
};
```

​	main.js

```javascript
import { createApp } from "vue";
import App from "./App.vue";

createApp(App).mount("#app");
```



#### 热更新(devServer)

* 方式一：添加`watch`选项

```javascript
module.exports = {
  entry: "./main.js",
  output: {
    path: `${__dirname}/dist`,
    filename: "[name]-boundle.js",
    clean: true,
  },
  watch: true,
}
```

* 方式二：使用`webpack-dev-server`

  安装`webpack-dev-server`

```javascript
//package.json
 "start": "webpack serve --open"
```

​	static.directory 为静态资源目录，当找不到资源时，会从改选项设置的文件下去找

```javascript
static: {
      directory: path.join(__dirname, "public"),
    },
```





#### 模块热替换

* 又称HMR: 不重新加载整个页面，只更新需要更新的部分，可以保留应用的状态不丢失

```javascript
//开启HMR
module.exports = {
   target: "web",
  devServer: {
    hot: true
  }
}
```

