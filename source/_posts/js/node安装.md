---
title: node安装
date: 2017-07-01 20:47:18
categories: [js]
tags: [js,node]
---
mac下node安装
<!-- more -->

<h1>删除node</h1>

```bash
brew uninstall nvm

brew uninstall node

rm ~/.nvm

rm /usr/local/bin/node

/usr/local/lib/

```

<h1>安装nvm</h1>

安装nvm，终端中输入
```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.1/install.sh | bash
```

在.bash_profile中写入环境配置
```bash
########node#########
export PATH="$PATH:$HOME/.rvm/bin" # Add RVM to PATH for scripting
export NVM_DIR="/Users/shirly/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm
########node#########
```

应用配置，终端中输入
```bash
source .bash_profile
```

<h1>nvm常用命令</h1>

```bash
nvm install <version> ## 安装指定版本，可模糊安装，如：安装v4.4.0，既可nvm install v4.4.0，又可nvm install 4.4
nvm uninstall <version> ## 删除已安装的指定版本，语法与install类似
nvm use <version> ## 切换使用指定的版本node
nvm ls ## 列出所有安装的版本
nvm ls-remote ## 列出所以远程服务器的版本（官方node version list）
nvm current ## 显示当前的版本
nvm alias <name> <version> ## 给不同的版本号添加别名
nvm unalias <name> ## 删除已定义的别名
nvm reinstall-packages <version> ## 在当前版本node环境下，重新全局安装指定版本号的npm包
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
