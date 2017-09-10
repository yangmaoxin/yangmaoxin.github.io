title: 自动备份与恢复Hexo博客源文件
date: 2016-09-01 12:08:51
categories: 教程
tags: [hexo]
---
<blockquote class="blockquote-center">此文记录自动备份Hexo博客源文件到Github，并且从Github恢复到新电脑的过程。</blockquote>
<!--more-->

# 自动备份Hexo博客源文件
## 原理
通过通过监听Hexo的其它事件来完成自动执行Git命令完成自动备份。
通过查阅Hexo文档，找到了Hexo的主要事件，见下表：

| 事件名         | 事件发生时间   | 
| --------   | -----:  | 
| deployBefore     | 在部署完成前发布 |  
| deployAfter        |   在部署成功后发布   |   
| exit        |    在 Hexo 结束前发布    |  
| generateBefore     | 在静态文件生成前发布 |  
| generateAfter        |   在静态文件生成后发布   |   
| new        |    在文章文件建立后发布    |  

于是我们就可以通过监听Hexo的deployAfter事件，待上传完成之后自动运行Git备份命令，从而达到自动备份的目的。

## 实现

将Hexo目录加入Git仓库
1. 在Github下创建一个新的repository，取名为HEXO。(与本地的Hexo源码文件夹同名即可)进入本地的Hexo文件夹，执行以下命令创建仓库:
```
git init
```
1. 设置远程仓库地址，并更新:
```
git remote add origin git@github.com:XXX/XXX.git git pull origin
master
```
1. 修改`.gitignore`文件（如果没有请手动创建一个），在里面加入`*.log` 和 `public/` 以及.`deploy*/`。因为每次执行`hexo generate`命令时，上述目录都会被重写更新。因此忽略这两个目录下的文件更新，加快push速度。
2. 执行命令以下命令，完成Hexo源码在本地的提交:
```
git add .
git commit -m "添加hexo源码文件作为备份"
```
1. 执行以下命令，将本地的仓库文件推送到Github:
```
git push origin master
```
### 安装shelljs模块
要实现这个自动备份功能，需要依赖`Node.js`的一个`shelljs`模块,该模块重新包装了child_process,调用系统命令更加的方便。该模块需要安装后使用。

在命令中键入以下命令，完成shelljs模块的安装：
```
npm install --save shelljs
```
### 编写自动备份脚本

待到模块安装完成，在Hexo根目录的scripts文件夹下新建一个js文件，文件名随意取。

ps: 如果没有scripts目录，请新建一个。

然后在脚本中，写入以下内容：
```
require('shelljs/global');

try {
    hexo.on('deployAfter', function() {//当deploy完成后执行备份
        run();
    });
} catch (e) {
    console.log("产生了一个错误<(￣3￣)> !，错误详情为：" + e.toString());
}

function run() {
    if (!which('git')) {
        echo('Sorry, this script requires git');
        exit(1);
    } else {
        echo("======================Auto Backup Begin===========================");
        cd('F:/geeknote');    //此处修改为Hexo根目录路径
        if (exec('git add --all').code !== 0) {
            echo('Error: Git add failed');
            exit(1);

        }
        if (exec('git commit -am "Form auto backup script\'s commit"').code !== 0) {
            echo('Error: Git commit failed');
            exit(1);

        }
        if (exec('git push origin master').code !== 0) {
            echo('Error: Git push failed');
            exit(1);

        }
        echo("==================Auto Backup Complete============================")
    }
}
```
其中，需要修改第17行的D:/hexo路径为Hexo的根目录路径。（脚本中的路径为博主的Hexo路径）

如果你的Git远程仓库名称不为origin的话，还需要修改第28行执行的push命令，修改成自己的远程仓库名和相应的分支名。

保存脚本并退出，然后执行hexo deploy命令，将会得到类似以下结果:
```
INFO  Deploying: git>
INFO  Clearing .deploy folder...
INFO  Copying files from public folder...
[master 3020788] Site updated: 2015-07-06 15:08:06
 5 files changed, 160 insertions(+), 58 deletions(-)
Branch master set up to track remote branch gh-pages from git@github.com:smilexi
amo/notes.git.
To git@github.com:smilexiamo/notes.git
   02adbe4..3020788  master -> gh-pages
On branch master
nothing to commit, working directory clean
Branch master set up to track remote branch gitcafe-pages from git@gitcafe.com:s
milexiamo/smilexiamo.git.
To git@gitcafe.com:smilexiamo/smilexiamo.git
   02adbe4..3020788  master -> gitcafe-pages
INFO  Deploy done: git
======================Auto Backup Begin===========================
[master f044360] Form auto backup script's commit
 2 files changed, 35 insertions(+), 2 deletions(-)
 rewrite db.json (100%)
To git@github.com:smilexiamo/hexo.git
   8f2b4b4..f044360  master -> master
==================Auto Backup Complete============================
```
这样子，每次更新博文并deploy到服务器上之后，备份就自动启动并完成备份啦

# 恢复Hexo博客源文件

切换电脑以后，在新电脑上安装node、git环境，配置github sshkey

创建空目录作为hexo工作目录，从远程仓库中clone出之前备份的repo
```
git clone git@github.com:xxx/geeknote.git
```

git clone成功后，本地出现geeknote文件夹，开始安装hexo环境
```
cd geeknote
npm install hexo
npm install
npm install hexo-deployer-git
npm install --save shelljs
```

之后执行
```
hexo g
hexo server
```

查看本地同步效果，访问localhost:4000

注意：
**这里不会备份hexo主题文件**

更新历史

* 2016年9月1日 13:30 增加恢复Hexo博客源文件


> 参考阅读
> 
> * [自动备份Hexo博客源文件][1] (原博主网站已经无法打开)
> * [自动备份Hexo博客源文件][2]


  [1]: http://notes.xiamo.tk
  [2]: http://zhujiegao.com/2015/12/06/automatic-backup/