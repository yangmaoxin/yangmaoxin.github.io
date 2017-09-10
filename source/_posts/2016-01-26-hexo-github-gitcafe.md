title: 把Hexo同时部署到GitHub和GitCafe
date: 2016-01-26 10:41:48
categories: 教程
tags: [hexo]
---
## 尝试
作为一个长期处在高墙内的中国人，在把Hexo部署在Github Pages上后，要时刻考虑如果Github也被丧心病狂地墙掉后，作为一个常用中文的博客，不能被墙内访问，多么寂寞。所以，尝试找个墙内的备胎Gitcafe。Gitcafe 也提供 Pages 服务，也官方支持 Hexo，实在是一个尽职的备胎。
<!--more-->
## 设置GitCafe
注册以后，创建一个项目，项目名需要与注册的用户名相同，默认分支选择gitcafe-pages,项目主页也是相同的。

## 修改_config.yml
修改根目录下的_config.yml文件中的deploy
根据Hexo官方文档需要修改成下面的形式
>deploy:
  type: git
  message: [message]
  repo:
    github: <repository url>,[branch]
    gitcafe: <repository url>,[branch] 

例如我的是这样
```
deploy:
  type: git
  message: ""
  repo: 
    github: git@github.com:yangmaoxin/yangmaoxin.github.io.git,master
    gitcafe: git@gitcafe.com:ymx/ymx.git,gitcafe-pages
```
将自己在Github和Gitcafe上的git填进去

## 我遇到的问题
部署到线上的时候总是提示
> Everything up-to-date

由于对git操作不是太了解，尝试把根目录下的.deploy文件夹删除。
然后重新执行一遍hexo clean hexo g hexo d一系列命令,就会成功提交。

## 万网域名双线解析
进入万网/阿里云后台，域名解析：

![域名解析][1]

添加两条CNAME，分别解析，解析路线选择默认至GitCafe，海外IP至GitHub。


  [1]: http://7xkrpj.com1.z1.glb.clouddn.com/%E9%98%BF%E9%87%8C%E4%BA%91%E8%A7%A3%E6%9E%90.png