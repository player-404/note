# nginx 入门

[Nginx入门](https://juejin.cn/post/6844903701459501070)

### 是什么？

> 是异步框架的[网页服务器](https://zh.wikipedia.org/wiki/網頁伺服器)，也可以用作[反向代理](https://zh.wikipedia.org/wiki/反向代理)、[负载平衡器](https://zh.wikipedia.org/wiki/负载均衡)和[HTTP缓存](https://zh.wikipedia.org/wiki/HTTP缓存) ( [Nginx - 维基百科)](https://zh.wikipedia.org/wiki/Nginx))



### 安装

1. 安装依赖

   ```
   dnf -y install gcc gcc-c++ autoconf pcre-devel make automake
   dnf -y install wget httpd-tools vim
   ```

2. 安装nginx源

   1. 检查本机是否存在源	

      `yum list | grep nginx`

      如果本机的yum源的nginx版本过老，可以进行替换

   2. 替换yum中的nginx源

      [nginx: Linux packages](http://nginx.org/en/linux_packages.html)

      找到网站中提供的yum源

      ```
      [nginx-stable]
      name=nginx stable repo
      baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
      gpgcheck=1
      enabled=1
      gpgkey=https://nginx.org/keys/nginx_signing.key
      module_hotfixes=true
      ```

      添加新的nginx yum源

      * centeros yum源管理目录: /etc/yum.repos.d

      * 添加新的源

        ```
         cd /etc/yum.repos.d
         vim nginx.repo
         
        # nginx.repo 文件中复制新的nginx源
        [nginx-stable]
        name=nginx stable repo
        baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
        gpgcheck=1
        enabled=1
        gpgkey=https://nginx.org/keys/nginx_signing.key
        module_hotfixes=true
        ```

      3. 下载：

      ​	`yum insall nginx`

      

### 配置详解

1. nginx 配置文件

   `/etc/nginx/nginx.conf`

   

   主配置项(`/etc/nginx/nginx.conf`)：

   ```
   user  nginx;
   #进程数
   worker_processes  auto; 
   #错误日志存放目录
   error_log  /var/log/nginx/error.log notice;
   pid        /var/run/nginx.pid;
   
   #允许最大的并发数
   events {
       worker_connections  1024;
   }
   
   
   http {
       #文件扩展名映射表
       include       /etc/nginx/mime.types;
       #默认文件类型
       default_type  application/octet-stream;
       #设置日志模式
       log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                         '$status $body_bytes_sent "$http_referer" '
                         '"$http_user_agent" "$http_x_forwarded_for"';
       #访问日志
       access_log  /var/log/nginx/access.log  main;
       #高速传输模式
       sendfile        on;
       #减少网络报文
       #tcp_nopush     on;
       #超时时间
       keepalive_timeout  65;
       #压缩，减少前端访问页面时间
       #gzip  on;
       #子配置项
       include /etc/nginx/conf.d/*.conf;
   }
   
   ```

   

   子配置项(` /etc/nginx/conf/default.conf `):

   ```
   server {
       #配置监听的端口
       listen       80;
       #域名
       server_name  localhost;
   
       #access_log  /var/log/nginx/host.access.log  main;
   
       location / {
           #网站文件目录
           root   /usr/share/nginx/html; 
           #网站文件入口
           index  index.html index.htm;
       }
   
       #error_page  404              /404.html;
   
       # redirect server error pages to the static page /50x.html
       #错误访问页面
       error_page   500 502 503 504  /50x.html;
       location = /50x.html {
           root   /usr/share/nginx/html;
       }
   
       # proxy the PHP scripts to Apache listening on 127.0.0.1:80
       #
       #location ~ \.php$ {
       #    proxy_pass   http://127.0.0.1;
       #}
   
       # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
       #
       #location ~ \.php$ {
       #    root           html;
       #    fastcgi_pass   127.0.0.1:9000;
       #    fastcgi_index  index.php;
       #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
       #    include        fastcgi_params;
       #}
   
       # deny access to .htaccess files, if Apache's document root
       # concurs with nginx's one
       #
       #location ~ /\.ht {
       #    deny  all;
       #}
   }
   ```

   

   2. niginx 常用命令

   * 启动

     `nginx`

     `systemctl start nginx.service`

   * 停止

     `nginx -s quit`

     `nginx -s stop`

     `killall nginx`

     `systemctl stop nginx.service`

   * 重启

     `systemctl restart nginx.service`

   * 重载配置文件

     `nginx -s reload`

### 页面配置

* 错误页面配置（default.conf）

  ```
   错误页面      错误状态码         跳转页面
   error_page   500 502 503 504  /50x.html;  可以是url: https://www.baidu.com
   跳转页面所在路径(url时不需要配置)
   location = /50x.html {
          root   /usr/share/nginx/html;
      }
  ```

* 禁止某个ip访问(deny)

  ```javascript
  # default.conf
  location / {
          #网站文件目录
          root   /usr/share/nginx/html; 
          #网站文件入口
          index  index.html index.htm;
    			#禁止该ip访问
    			deny   xxxx.xxxx.xxxx.xxxx;
      }
  ```

* 允许某个ip访问(allow)

  ```javascript
  # default.conf
  location / {
          #网站文件目录
          root   /usr/share/nginx/html; 
          #网站文件入口
          index  index.html index.htm;
    			#允许该ip访问
    			allow  xxx.xxx.xxx.xxx;
    			#禁止该ip访问
    			deny   all;
      }
  ```



### 设置权限

> nginx 执行是从上往下执行的，上面的执行权重时大雨下面的权重的， 当两条语句冲突时，显然以上下文上面一条语句为准

* **设置目录访问权限**

  `=`	精确匹配符

  ```javascript
  #img目录允许任何人访问
  location =/img {
  	allow all;
  }
  
  #admin目录禁止任何人访问
  location =/damin {
  	deny all;
  }
  
  ```

  `~`   正则表达式

  ```javascript
  #禁止访问某php页面
  location ~\.php$ {
   deny all;
  }
  ```



### 设置虚拟主机

> 一台服务器起多个nginx服务，以提供不同的服务

* **基于端口号创建虚拟机**

```javascript
# 新建conf/8001.conf配置文件
# server 类似一个主机
server {
  listen 8001; # server监听的端口号
  server_name localhost; # server 域名
  root /usr/share/nginx/html. # web 文件路径
  index index.html; # 入口文件
}
```

* **基于域名**

  ```javascript
  # 新建conf/8001.conf配置文件
  server {
    listen: 80;  # server监听的端口号
    server_name zphlplay.top # server 域名
    root /usr/share/nginx/html. # web 文件路径
    index index.html; # 入口文件
  }
  ```

  

### 反向代理(proxy_pass)

```
server{
        listen 80;
        server_name nginx2.jspang.com;
        location / {
               proxy_pass http://jspang.com;
        }
}

```

**其它反向代理指令**

反向代理还有些常用的指令：

- proxy_set_header :在将客户端请求发送给后端服务器之前，更改来自客户端的请求头信息。
- proxy_connect_timeout:配置Nginx与后端代理服务器尝试建立连接的超时时间。
- proxy_read_timeout : 配置Nginx向后端服务器组发出read请求后，等待相应的超时时间。
- proxy_send_timeout：配置Nginx向后端服务器组发出write请求后，等待相应的超时时间。
- proxy_redirect :用于修改后端服务器返回的响应头中的Location和Refresh。



### 适配设备

```javascript
server {
	listen 80;
	server_name zphiplay.top;
	location / {
		root /user/share/html/pc.html; # pc文件目录
		if ($http_user_agent ~* 'Android|webOS|iphone|ipod|BlackBeery') {
			root /usr/share/html/mobile.html; # mobile文件目录
		}
		index index.html
	}
}
```





### Gzip压缩

```javascript
# nginx.conf文件
http {
	gzip on; 开启gzip压缩
	...
}
 
  # 参考配置
gzip on; # 开启Gzip
 
gzip_min_length 1k; # 不压缩临界值，大于1K的才压缩，一般不用改
 
gzip_buffers 4 16k; # 设置用于处理请求压缩的缓冲区数量和大小。比如32 4K表示按照内存页（one memory page）大小以4K为单位（即一个系统中内存页为4K），申请32倍的内存空间。建议此项不设置，使用默认值。
 
#gzip_http_version 1.0; # 用了反向代理的话，末端通信是HTTP/1.0，有需求的应该也不用看我这科普文了；有这句的话注释了就行了，默认是HTTP/1.1
 
gzip_comp_level 2; # 压缩级别，1-10，数字越大压缩的越好，时间也越长，看心情随便改吧
 
gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png; # 进行压缩的文件类型，缺啥补啥就行了，JavaScript有两种写法，最好都写上吧，总有人抱怨js文件没有压缩，其实多写一种格式就行了
 
gzip_vary off; # 跟Squid等缓存服务有关，on的话会在Header里增加"Vary: Accept-Encoding"，按需开启
 
gzip_disable "MSIE [1-6]\."; # IE6对Gzip不怎么友好，不给它Gzip了
```

Nginx提供了专门的gzip模块，并且模块中的指令非常丰富。

- gzip : 该指令用于开启或 关闭gzip模块。
- gzip_buffers : 设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。
- gzip_comp_level : gzip压缩比，压缩级别是1-9，1的压缩级别最低，9的压缩级别最高。压缩级别越高压缩率越大，压缩时间越长。
- gzip_disable : 可以通过该指令对一些特定的User-Agent不使用压缩功能。
- gzip_min_length:设置允许压缩的页面最小字节数，页面字节数从相应消息头的Content-length中进行获取。
- gzip_http_version：识别HTTP协议版本，其值可以是1.1.或1.0.
- gzip_proxied : 用于设置启用或禁用从代理服务器上收到相应内容gzip压缩。
- gzip_vary : 用于在响应消息头中添加Vary：Accept-Encoding,使代理服务器根据请求头中的Accept-Encoding识别是否启用gzip压缩。



更多参考

[Nginx gzip参数详解及常见问题](https://www.cnblogs.com/xzkzzz/p/9224358.html)

