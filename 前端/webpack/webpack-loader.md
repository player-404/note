#### loader

loader 的相关设置在`options`选项中，也可单独写在配置文件中

* style-loader: 将css插入head中

* css-loader: 将css转换为js

* postcss-loader: 处理css兼容

  `postcss-preset-env`: 该插件处理不同浏览器之间的兼容, 需在`options`中配置

* babel-loader: 处理js兼容

  `@babel/preset-env`: 将es6语法转换为兼容语法