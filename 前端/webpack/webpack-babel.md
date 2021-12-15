# babel打包JavaScript

* 作用

​	babel主要用作打包js，将ES6+语法打包向后兼容的语法

* 相关包

​	**@babel/core**: babel的核心

​	**@babel/cli**: 能够在命令行使用babel

​	**@babel/preset-env**: 转换ES6+语法

​	**@babel/polyfill**: 转换ES6+的api		

​	**@babel/plugin-transform-runtime**: 避免一个包被多个地方重新引用，减少包的体积

```javascript
module: {
    rules: [
      {
        test: /\.js$/i,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"],
            plugins: ["@babel/plugin-transform-runtime"], // 解决包重复引用的问题
          },
        },
      },
    ],
  },
```

