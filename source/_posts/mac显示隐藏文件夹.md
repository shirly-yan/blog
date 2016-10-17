---
title: mac显示隐藏文件夹
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

打开终端，输入：defaults write com.apple.finder AppleShowAllFiles -bool true 此命令显示隐藏文件defaults write com.apple.finder AppleShowAllFiles -bool false 此命令关闭显示隐藏文件
命令运行之后需要重新加载Finder：快捷键option+command+esc，选中Finder，重新启动即可