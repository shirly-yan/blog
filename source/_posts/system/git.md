---
title: git
date: 2016-06-01 10:10:10
categories: software 
tags: git
---
git
<!-- more -->

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email your.email@example.com
```

<h3>初始化一个新仓库，init命令</h3>
```bash
$ git init
```

<h3>把项目中的文件放到仓库中，add命令</h3>
-A 旗标的意思是所有文件
```bash
$ git add -A
```

<h3>查看暂存区中有哪些文件，status命令</h3>
```bash
$ git status
```

<h3>保存这些改动，commit命令</h3>
-m 旗标的意思是为这次提交添加一个说明
```bash
git commit -m "Initialize repository"
```

<h3>查看提交历史，log命令</h3>
```bash
$ git log
```
<h3>设置远程仓库地址</h3>
```bash
$ git remote add origin git@bitbucket.org:<username>/hello_app.git
```
<h3>推送</h3>
```bash
$ git push -u origin --all
```

<h2>分支</h2>

<h3>列出所有本地分支</h3>
```bash
$ git branch 
```

<h3>命令先创建一个新分支，然后再切换到这个新分支</h3>
```bash
$ git checkout -b branch_name
```

<h3>提交现有文件中的全部改动</h3>
```bash
$ git commit -a -m "Improve the README file"
```

<h3>删除分支: git branch -d branch_name</h3>
```bash
$ git branch -d modify-README
```

<h3>放弃主题分支中的改动: git branch -D branch_name</h3>
 
```bash
$ git checkout -b topic-branch
$ <really screw up the branch>
$ git add -A
$ git commit -a -m "Make major mistake"
$ git checkout master
$ git branch -D topic-branch
```

<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
