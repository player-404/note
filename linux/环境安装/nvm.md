# node版本管理工具(nvm)

[nvm-sh/nvm: Node Version Manager - POSIX-compliant bash script to manage multiple active node.js versions (github.com)](https://github.com/nvm-sh/nvm)

### 安装nvm

```javascript
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

```



### node管理

```javascript
# 最新的lts版本
nvm install --lts
//or
nvm install node
```





### mac安装

## 前言

在使用 `node` 的过程中，用 `npm` 安装一些模块，特别是全局包的时候，由于 `Mac` 系统安全性的限制，经常出现安装没有权限，或者安装完成使用时出现 `Command not found` 的情况。

之前我都是通过使用修改权限的方式来解决，但是太麻烦又感觉不太安全，于是我就到网上找解决的方法，发现其实官方也是推荐我们使用 `node` 的管理工具来解决这个问题的。官方推荐了两个 `n` 和 `nvm`，这里我选择的是 `nvm`。

至于两者的区别可以看一下淘宝团队的一篇文章[管理node版本，选择nvm还是n？](https://link.segmentfault.com/?url=https%3A%2F%2Ffed.taobao.org%2Fblog%2Ftaofed%2Fdo71ct%2Fnvm-or-n%2F%3Fspm%3Dtaofed.homepage.header.7.7eab5ac8dDRkRS)

## 安装

这篇文章主要想说的就是在 `Mac` 下 `nvm` 的安装和遇到的问题。

> 注意：不要使用 `Homebrew` 安装 `nvm`，这个在 `nvm` 的官方文档中有说明。

![nvm官方说明](https://segmentfault.com/img/bVbk8wz)

具体的步骤如下：

首先打开终端，进入当前用户的 `home` 目录中。

```
cd ~/
```

然后使用 `ls -a` 显示这个目录下的所有文件（夹）（包含隐藏文件及文件夹），查看有没有 `.bash_profile` 这个文件。

```
ls -a
```

如果没有，则新建一个。

```
touch ~/.bash_profile
```

如果有或者新建完成后，我们通过官方的说明在终端中运行下面命令中的一种进行安装：

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
```

在安装完成后，也许你会在终端输入 `nvm` 验证有没有安装成功，这个时候你会发现终端打出 `Command not found`，其实这并不是没有安装成功，你只需要重启终端就行，再输入 `nvm` 就会出现 `Node Version Manager` 帮助文档，这表明你安装成功了。

## 注意

这里需要注意的几点就是：

第一点 不要使用 `homebrew` 安装 `nvm`

第二点 关于 `.bash_profile` 文件。如果用户 `home` 目录下没有则新建一个就可以了，不需要将下面的两段代码写进去，因为你在执行安装命令的时候，系统会自动将这两句话写入 `.bash_profile` 文件中。

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

网络上我找了好多文章都是说在安装前先手动将下面这两句话写进去，经过测试是不正确的，并且会造成安装不成功，这一点需要注意一下。

```
export NVM_DIR="${XDG_CONFIG_HOME/:-$HOME/.}nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

第三点 保证 `Mac` 中安装了 `git`，一般只要你下载了 `Mac` 的 `Xcode` 开发工具，它是自带 `git` 的。

## 在最新的 Catalina 系统下安装 nvm

注意最新的 `macOS Catalina` 系统（即版本 10.15 及之后）默认的 `shell` 是 `zsh`，不在是 `bash` ，安装完之后会出现命令不可用的情况。

这里有两种解决办法：

第一种 我们需要将默认 `shell` 改为 `bash`，具体参见苹果官网的相关文章 [如何更改默认 Shell](https://link.segmentfault.com/?url=https%3A%2F%2Fsupport.apple.com%2Fzh-cn%2FHT208050)，如果你之前就习惯使用 `zsh` 也可自行配置。

第二种 如果你要使用 `zsh` 终端，那么在上述方式安装完之后，在 `.bash_profile` 同一目录下创建一个 `.zshrc` 文件，使用 `vim` 打开文件添加下面两句话，重启终端即可。

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

## 使用 git 安装方式来解决 443 等错误

最近买了新电脑，重新配置开发环境时，发现在安装 `nvm` 时一直出现443错误，之前只是偶尔出现。网上有很多解决的办法，但是我一直坚持保持系统的干净整洁，不想过多的更改系统文件和配置（正是坚持这种原则，每次在环境搭建之后，整个流程一直都很稳定，没有出现过一些奇怪的问题），最后查阅资料发现用 `git` 安装的方式可以解决问题，方法如下：

> 注意：
>
> 需要 `git` 版本 v1.7.10+
> 除了不再使用 `curl` 或 `wget` 方式安装之外，之前所有的注意事项和步骤依然有效

首先，进入当前用户的 `home` 目录，并 `clone`

```
cd ~/
git clone https://github.com/nvm-sh/nvm.git .nvm
```

`clone` 完成后，进入 `.nvm` 目录，并 `checkout` 最新版本

```
cd ~/.nvm
git checkout v0.38.0
```

然后，我们来激活它

```
. ./nvm.sh
```

注意使用 `git` 方式安装 `nvm` 就需要将下面两句话手动写入 `.bash_profile` 文件中，否则就会出现重启 `bash` 之后命令找不到的情况

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

最后在终端输入 `nvm` 来看验证是否安装成功

## 一些常见的nvm命令

`nvm ls-remote` 列出所有可安装的版本

```
nvm install <version>` 安装指定的版本，如 `nvm install v8.14.0
```

`nvm uninstall <version>` 卸载指定的版本

`nvm ls` 列出所有已经安装的版本

`nvm use <version>` 切换使用指定的版本

`nvm current` 显示当前使用的版本

`nvm alias default <version>` 设置默认 `node` 版本

`nvm deactivate` 解除当前版本绑定

> nvm 默认是不能删除被设定为 default 版本的 node，特别是只安装了一个 node 的时候，这个时候我们需要先解除当前版本绑定，然后再使用 `nvm uninstall <version>` 删除

## node被安装在哪里

在终端我们可以使用 `which node` 来查看我们的 `node` 被安装到了哪里，这里终端打印出来的地址其实是你当前使用的 `node` 版本快捷方式的地址。

```
/Users/你的用户名/.nvm/versions/node/v10.13.0/bin/node
```

如果你想查看所有 `node` 版本的安装文件夹，我们可以在 `访达（finder）` 中使用快捷键 `Command+Shift+G` 输入 `/Users/你的用户名/.nvm/versions` 地址就可以看到。

但是这里想说的是 `Mac` 默认是不显示隐藏文件夹的，`.nvm` 是个隐藏文件夹在 `访达（finder）` 中看不到，在 `Mac` 下显示隐藏文件的快捷键是 `Command+Shift+.`，关闭也是这个快捷键。

看！这是我的！

![node位置](https://segmentfault.com/img/bVbk8AE)

这是在 `v8.14.0` 版本 `node` 的 `lib` 中 `node_modules` 下的模块，可以看到我刚刚安装的 `cnpm` 和 `electron`

![v8.14.0](https://segmentfault.com/img/bVbk8AP)

但是在 `v10.13.0` 同样的目录下只有 `npm`

![v10.13.0](https://segmentfault.com/img/bVbk8AU)

所以可以知道在 `nvm` 下 `node` 版本管理方式，安装的模块不是公用的，也就是说你在切换版本后需要在切换的版本下重新安装，这一点和 `n` 是不同的，当然这也是它的优势。

