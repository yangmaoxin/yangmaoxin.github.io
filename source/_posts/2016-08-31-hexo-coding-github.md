title: 把Hexo同时部署到GitHub和Coding
date: 2016-08-31 20:15:54
categories: 教程
tags: [hexo]
---
<blockquote class="blockquote-center">因为Gitcafe已经被Coding收购，修改同时更新到Github以及Coding。</blockquote>
<!--more-->
## 设置Coding
在Coding上创建一个项目，项目名最好与注册时的账户用户名相同，这样比较方便。在之后使用Coding Pages可以通过 `{用户名}.coding.me` 形式的 URL 直接访问。

## 设置SSH
如果是第一次使用coding的话，需要设置SSH公钥，生成的方法可以参考[coding帮助中心][1]

之前我在部署Github时已经配置过Github的公钥，所以这里直接使用。
![ssh目录][2]


本地文件夹中存在了，一般存放在`~/.ssh/id_rsa.pub`。复制这个文件夹下的`id_rsa.pub` 内容添加在coding的个人页面内，如下
![1][3]

> 注意这是在个人账户下添加了ssh,以后新建了项目，项目也有个ssh，但是那个是只读的，不要加到那里去。只需要在个人账户下，加入了即可。

验证是否连接成功:

`ssh -T git@git.coding.net`

提示如下即可：
`Hello iymx You've connected to Coding.net by SSH successfully!`



## 修改_config.yml
修改Hexo根目录下的_config.yml文件中的deploy
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
    coding: git@git.coding.net:iymx/iymx.git,master
```
将自己在Github和Coding上的git填进去

最后使用部署命令就能把博客同步到coding上面：
`hexo deploy -g`

## Coding Pages服务方式部署

和Github Pages一样。项目名字必须与Coding的用户名一致。

> 在source/需要创建一个空白文件，至于原因，是因为
> coding.net需要这个文件来作为以静态文件部署的标志。就是说看到这个Staticfile就知道按照静态文件来发布。

    cd source/
    touch Staticfile  #名字必须是Staticfile

只需要在这里开启Page
![codingpages][4]

因为前面配置的分支是master,因此需要部署分支master。


如果项目名和Coding的用户名一样，比如我的用户是iyxm,博客项目名也是iymx
那直接访问  [iymx.coding.me][5]     
就能访问博客，否则就要带上项目名：iymx.coding.me/项目名 才能访问
推荐项目名跟用户名一样，这样就可以省略项目名。

## 万网域名双线解析
进入万网/阿里云后台，域名解析：

![域名解析][6]

添加两条CNAME，分别解析，解析路线选择默认至Coding，海外IP至GitHub。


  [1]: https://coding.net/help/doc/git/ssh-key.html
  [2]: http://photo.yangmaoxin.cn/addssh.png
  [3]: http://photo.yangmaoxin.cn/coding1.png
  [4]: http://photo.yangmaoxin.cn/codingpages.png
  [5]: http://igeek.wang
  [6]: http://photo.yangmaoxin.cn/githubcoding.png