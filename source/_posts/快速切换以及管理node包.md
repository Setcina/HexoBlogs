---
title: 快速切换以及管理node包
date: 2019-07-20 09:51:44
tags:
  Work Experience
---

### 前言

为什么需要快速切换以及管理node包？在我们的日常工作，并不仅仅只是开发新项目，用到新技术，还有一些旧的项目需要维护。那么就有一个问题了，现在前端大部分的开发都是基于node提供的环境下，而node日新月异，难免会有新版本不支持旧版本的情况。所以，这就需要我们快速的切换以及管理node包，已达到开发新项目或者维护旧项目的工作要求

### nvm管理node包

nvm 是 Mac 下的 node 管理工具，有点类似管理 Ruby 的 rvm，如果是需要管理 Windows 下的 node，官方推荐是使用 nvmw 或 nvm-windows 。nvm主要用来在不同的nodejs版本中切换，以便当node出新版本时，可以使用一些新的特性

#### nvm安装以及运行
``` js
git clone https://github.com/nvm-sh/nvm.git ~/.nvm
```

运行nvm -v  如果此时提示 nvm command not found。那么就是缺少了.bash_profile
``` js
新建文件.bash_profile

vim .bash_profile
输入

source ~/.bashrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

```
上面的语句意思是，当你在使用nvm这个命令的时候，会帮你打开./nvm下面对应的nvm.sh以及bash_completion文件去执行。

查看命令
``` js
nvm --help
从这里查看nvm可以使用的命令
```

### 使用node版本管理模块n
安装node版本管理模板n
``` js
sudo npm install n -g
```
下面就可以按照自己的需求选择了
```
选择稳定版
sudo n stable

安装最新版
sudo n latest

版本降级/升级
sudo n 版本号

查看安装了那些版本的node
n

切换到9.6.0
n 9.6.0

删除版本
sudo n rm 版本号

```

