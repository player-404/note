# webpack插件

#### 1.DefinePlugin

* 定义值，该插件为内置插件

```javascript
plugins: [
  new webpack.DefinePlugin({
  // 定义...
});
]
```

模板中使用`<%= 变量 %>`读取定义的值

```html
html
 <span>url is <%= BASE_URL %></span>
 
webpack
<script>
	const { DefinePlugin } = require("webpack");
  
   plugins: [
    new DefinePlugin({
      BASE_URL: JSON.stringify("./"),
    }),
  ],
</script>
```



#### 2.CopyWebpackPlugin

复制文件到指定的位置(不会进行打包，复制原文件)

```javascript
// 复制public文件夹下的所有文件值打包目录的imgssss文件夹下
 new CopyWebpackPlugin({
      patterns: [
        {
          from: "public",
          to: "imgssss",
        },
      ],
    }),
   
   
  //复制单独的文件值指定的文件夹下
  new CopyWebpackPlugin({
      patterns: [
        {
          from: "./2.jpg",
          to: "imgssss",
        },
        {
          from: "public/3.jpg",
          to: "img1",
        },
      ],
    }),
```



#### 3.HtmlWebpackPlugin

打包html

```javascript
//html打包
    new HtmlWebpackPlugin({
      title: "vue app",
      //输出文件路径及文件名（不支持[name] [ext]等属性）
      filename: "index.html",
      //模板文件
      template: "./src/index.html",
      //引入文件放在body标签底部
      inject: "body",
    }),
```



