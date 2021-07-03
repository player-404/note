# git

### 源码安装（centeros 8）

1. 开启EPEL库

   `dnf config-manager --set-enabled powertools`

2. 下载必须依赖库

   `sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel \
     openssl-devel perl-devel zlib-devel`

3. 下载附加依赖

   `sudo dnf install asciidoc xmlto docbook2X`

4. 执行以下命令

   `sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi`

5. 编译并安装

   ```
   $ tar -zxf git-2.8.0.tar.gz
   $ cd git-2.8.0
   $ make configure
   $ ./configure --prefix=/usr
   $ make all doc info
   $ sudo make install install-doc install-html install-info
   ```

   









