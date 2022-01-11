# 版本

#### vue不同版本区别

![截屏2021-12-15 下午3.07.30](https://raw.githubusercontent.com/player-404/images/main/%E6%88%AA%E5%B1%8F2021-12-15%20%E4%B8%8B%E5%8D%883.07.30.png)



#### 打包Vue

​	webpack打包vue3：

​		loader: `vue-loader@next`

​		插件： `@vue/compiler-sfc`,`VueLoaderPlugin`

​	

​	完整配置:

​	webpack.config.js

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");
const CopyWebpackPlugin = require("copy-webpack-plugin");
const { DefinePlugin } = require("webpack");
const { VueLoaderPlugin } = require("vue-loader/dist/index");

module.exports = {
  target: "web",
  //入口
  entry: "./main.js",
  //出口
  output: {
    path: `${__dirname}/dist`,
    filename: "[name]-boundle.js",
    clean: true,
  },
  mode: "development",
  module: {
    rules: [
      //cssd打包
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
      //scss打包
      {
        test: /\.scss$/i,
        use: ["style-loader", "css-loader", "postcss-loader", "sass-loader"],
      },
      //图片打包
      {
        test: /\.(jpe?g|png|svg)$/i,
        //通用资源类型
        type: "asset",
        parser: {
          // 大小限制
          dataUrlCondition: {
            maxSize: 1024 * 30,
          },
        },
        //输出目录
        generator: {
          filename: "img/[name]-[hash][ext][query]",
        },
      },
      //打包js(兼容)
      {
        test: /\.js$/i,
        //排除文件
        exclude: /node_modules/,
        loader: "babel-loader",
      },
      //打包vue
      {
        test: /\.vue$/i,
        loader: "vue-loader",
      },
    ],
  },
  plugins: [
    // //html打包
    new HtmlWebpackPlugin({
      title: "vue app",
      //输出文件路径及文件名（不支持[name] [ext]等属性）
      filename: "index.html",
      //模板文件
      template: "./src/index.html",
      //引入文件放在body标签底部
      inject: "body",
    }),
    //复制文件到指定的位置
    new CopyWebpackPlugin({
      patterns: [
        {
          from: "src/public",
          to: "public",
        },
      ],
    }),
    new VueLoaderPlugin(),
    new DefinePlugin({
      __VUE_OPTIONS_API__: true,
      __VUE_PROD_DEVTOOLS__: false,
    }),
  ],
  devServer: {
    //压缩文件
    compress: true,
    port: 8080,
    //模块热替换
    hot: true,
    //监听指定文件变化
    watchFiles: ["src/**/*"],
    //自动打开浏览器
    open: true,
  },
};

```

> 注意：需要使用DefinePlugin将`__VUE_OPTIONS_API__`设置为true,  `__VUE_PROD_DEVTOOLS__`设置为false，否则会出现警告
>
> 经测试，不需要`@vue/compiler-sfc`插件

​	main.js

```javascript
import { createApp } from "vue";
import App from "./App.vue";

createApp(App).mount("#app");
```

index.html

```html
<body>
    <div id="app"></div>
</body>
```

> 需要在index.html中指定挂在id为app的元素



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
      //模块热替换
    hot: true
  }
}
```

> 注意：
>
> 启用该设置, 当源码更改后**不会自动刷新页面**，需要手动刷新
>
> 解决：
>
> 将该设置关闭或设置`watchFiles`

#### proxy

"/api": 代理到`http://localhost:8080`详细配置见官网

```javascript
proxy: {
      "/api": {
        target: "http://localhost:8080",
        secure: true,
        changeOrigin: true,
      },
    },
```



#### resolve(解析)

这些选项能设置模块如何被解析

* alias(路径别名)

​	创建 `import` 或 `require` 的别名，来确保模块引入变得更简单

```javascript
resolve: {
    alias: {
      //将符号@映射为src目录的绝对路径
      "@": path.resolve(__dirname, "src"),
    },
  },
```



* extensions

​	尝试按顺序解析这些后缀名。如果有多个文件有相同的名字，但后缀名不同，webpack 会解析列在数组首位的后缀的文件 并跳过其余的后缀(导入文件时，可以省略后缀名)

```javascript
// 以下文件在导入时，可以省略后缀名
    extensions: ["js", "vue", "ts", "jsx"],
```

