title: Hexo+Github，搭建属于自己的博客
date: 2016-01-25 12:14:06
categories: 教程
tags: [hexo]
---
## 前言
### 什么是GitHub Pages 
本是用于介绍托管在 GitHub 的项目，现很流行用来搭建博客，有300M免费空间。
### 什么是Hexo

> 快速、简单且功能强大的 Node.js 博客框架。 A fast, simple & powerful blog
> framework,powered by Node.js.

类似于jekyll、Octopress、Wordpress，我们可以用hexo创建自己的博客，托管到github或Heroku上，绑定自己的域名，用markdown写文章。
本博客就是使用hexo创建并托管在github上。

### 为什么要用hexo?
> 不可思议的快速 ─ 只要一眨眼静态文件即生成完成
支持 Markdown
仅需一道指令即可部署到 GitHub Pages 和 Heroku
已移植 Octopress 插件
高扩展性、自订性
兼容于 Windows, Mac & Linux

以上是引用作者的话。

对我而言，一个界面简洁，可以使用markdown写作，前期搭建的感觉很有B格。
所以记录一下搭建的过程以便之后回来方便查看。
<!--more-->

## 配置环境
### 安装Node
作用：用来生成静态页面的。
到Node.js官网下载相应平台的最新版本，一路安装即可。

### 安装Git
作用：把本地的hexo内容提交到github上去.

msysgit是Windows版的Git，从[这里][1]下载，然后按默认选项安装即可。具体查看[廖雪峰Git教程][2]

### 申请GitHub
作用：是用来做博客的远程创库、域名、服务器之类的。

github账号直接申请就行，跟一般的注册账号差不多，SSH Keys，可以不配制，不配置的话以后每次对自己的博客有改动提交的时候需要手动输入账号密码，配置后不需要密码，网上有很多教程。

### 配置SSH
使用hexo博客必须配置SSH。
打开git bash，输入`cd ~/.ssh`，如果果提示：No such file or directory 说明未配置SSH。

 - 本地生成密钥对
`ssh-keygen -t rsa -C "你的邮件地址"`，注意命令中的大小写不要搞混。按提示指定保存文件夹，不设置密码。
 -  添加公钥到Github
1. 根据上一步的提示，找到公钥文件（默认为id_rsa.pub），用记事本打开，全选并复制。
2. 登录Github，右上角 头像 -> `Settings` —> `SSH keys` —> `Add SSH` key。把公钥粘贴到key中，填好title并点击 `Add key`。
3. git bash中输入命令`ssh -T git@github.com`，选yes，等待片刻可看到成功提示。
 - 修改本地的ssh remote url，不用https协议，改用git协议
1. Github仓库中获取ssh协议相应的url
2. 本地仓库执行命令`git remote set-url origin SSH`对应的url，配置完后可用`git remote -v`查看结果

这样`git push`或`hexo d`时不再需要输入账号密码。

## hexo安装和使用
以下命令行需要在Git终端中执行。

 -  创建一个新文件夹作为hexo的安装目录，这样hexo文件都在里面，主要是为了方便管理。
 -  在这个文件夹内，右键打开git bash(这里是以windows系统为例。)。注意：最好使用管理员身份打开。
 -  安装hexo
```
npm install -g hexo #可用hexo -v查看版本。这里我用的是3.1.1。 
npm update hexo -g #升级 
```
 -  布置hexo
```
hexo init #初始化
```
hexo 会在目标文件夹建立网站所需要的所有文件。

 -  安装依赖包：npm install
 -  创建Github Repository：Repository名字必须是你的Github名.github.io，比如我是yangmaoxin.github.io
 -  部署：打开博客根目录下的_config.yml文件，末尾添加如下信息。
```
deploy:
  type: git
  repository: 上一步的Github仓库地址，项目主页点SSH再复制URL
  branch: master
```
然后执行命令：
```
hexo generate # 生成静态页面，可以简化为hexo g
hexo deploy # 部署到Github，可以简化为hexo d
```
浏览器访问yangmaoxin.github.io就能看到自己的Blog了，一般延迟十分钟左右才能看到效果。一开始看到404页面不要惊慌，耐心等等。

## hexo常用命令
### 简写
```
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo p == hexo publish
hexo g == hexo generate#生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy#部署
```
### 服务器
```
hexo server #Hexo 会监视文件变动并自动更新，您无须重启服务器。
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP

hexo clean #清除缓存 网页正常情况下可以忽略此条命令
hexo g #生成静态网页
hexo d #开始部署
```
### 监视文件变动
```
hexo generate #使用 Hexo 生成静态文件快速而且简单
hexo generate --watch #监视文件变动
```
### 完成后部署    
```
#以下两个命令的作用是相同的
hexo generate --deploy
hexo deploy --generate
hexo deploy -g
hexo server -g
```
### 草稿

    hexo publish [layout] <title>

### 模板
```
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub

hexo new [layout] <title>
hexo new photo "My Gallery"
hexo new "Hello World" --lang tw
```
更多的hexo命令可以到[这里][3]查看


  [1]: https://git-for-windows.github.io
  [2]: http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000
  [3]: http://segmentfault.com/a/1190000002632530#articleHeader10