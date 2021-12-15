# webpack配置选项

* devtool

​	devtool用作调试，在找错误方面给了我们很大的帮助

![截屏2021-12-13 上午8.57.56](../../../../Library/Application Support/typora-user-images/截屏2021-12-13 上午8.57.56.png)

​    关键字

​    eval, source-map， cheap,  module 最常见关键字就是这几个，最后几个配置（inline, hidden, nosource下文讲），很多配置就是这几个组合起来的，那么这几个关键字是干嘛的，我们一一解释

​    eval: 

​    这个关键字的意思是说， 每个模块用eval执行，并且存在@sourceUrl，就是说这种配置的devtool，在打包的时候，生成的bundle.js文件，模块都被eval包裹，并且后面跟着sourceUrl,指向的是原文件index.js，调试的时候，就是根据这个sourceUrl找到的index.js文件的。

​    source-map:

​    这种配置会生成一个带有.map文件，这个map文件会和原始文件做一个映射，调试的时候，就是通过这个.map文件去定位原来的代码位置的，

​    cheap:

​    这个意思是说，低消耗打包，什么叫低消耗，就是打包的时候map文件，不会保存原始代码的列位置信息，只包含行位置信息，所以这就解释官网图后面的说明(仅限行)，可能这样说还有点不理解我们看看两张图就知道了.
 第一张是设置devtool: 'source-map'的结果



* mode

   设置webpack的环境，不同的环境，webpack会自动配置一些不同的设置，常用的配置有`production`与`development`