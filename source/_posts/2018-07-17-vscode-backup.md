title: VSCode 配置备份与同步
date: 2018-07-17 11:01:39
categories: 学习笔记
tags: [效率,备份]
---
<blockquote class="blockquote-center">一个非常有用的 VSCode 插件Settings Sync，备份和同步 VSCode 的设置，操作系统和多端同步不用再折腾 VSCode 配置。</blockquote>
<!--more-->
## 安装插件

在扩展商店中搜索并安装插件 **setting sync** 
> **setting sync** 是vscode中同步设置和安装插件的小工具


## 利用github帐户进行备份

1. 登陆github>用户头像 > settings > developers > Personal access tokens > Generate new token

2. 输入描述比如vscode ,勾选gist,> generate token

3. 保存生成的token。

## 在vscode 上配置

1. 快捷键 shift+alt+u 或 ctrl+p 输入>sync点击update/updload settings

2. 把之前复制的access token粘贴后回车

3. 成功后跳出如下图，复制GITHUB GIST的内容，在需要同步的另一台电脑上使用

## 获取gist id

1. 先进入到： 

    `https://gist.github.com/<username> `

2. 点击你的gist文件,url上的最后的参数就是gist id

   `https://gist.github.com/<username>/<gist id>`

## 恢复备份

1. 在需要同步的电脑打开VSCode,安装相同的插件

2. 按快捷键 shift+alt+d 或 ctrl+p 输入>sync点击Download Settings

3. 把GITHUB GIST的gist id和内容粘贴然后回车