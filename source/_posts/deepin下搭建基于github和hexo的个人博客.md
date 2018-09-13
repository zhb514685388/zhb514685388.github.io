---
title: deepin下搭建基于github和hexo的个人博客
date: 2018-09-13 21:11:19
tags:
---

搭建步骤： 
1、 安装git

$ sudo apt-get install git 

查看git版本

$ git version 
<!--more-->
2、 安装Node.js及npm 

a. 可以直接命令安装,但是命令安装的不是最新版本。

$ sudo apt-get install nodejs $ sudo apt-get install npm 

b. 本博客采用第二种方法，首先官网下载最新版，然后解压。将node,npm命令设置全局命令：

$ sudo ln -s /home/dudefu/Documents/node-v8.6.0-linux-x64/bin/node /usr/local/bin/node $ sudo ln -s /home/dudefu/Documents/node-v8.6.0-linux-x64/bin/npm /usr/local/bin/npm 1 2

查看版本：

$ node -v $ npm -v 

 3、 安装hexo

$ npm install -g hexo-cli 1 hexo-cli安装路径/home/dudefu/Documents/node-v8.6.0-linux-x64/lib/node_modules/hexo-cli，此时输入命令hexo会提示“未找到命令”，此时要将hexo-cli/bin/文件夹下的hexo命令设置为全局：

$ sudo ln -s /home/dudefu/Documents/node-v8.6.0-linux-x64/lib/node_modules/hexo-cli/bin/ $ hexo /usr/local/bin/hexo 1 2 再输入hexo命令可以正常显示。 创建一个空文件夹，此处名为hexo：

$ mkdir hexo $ cd hexo $ hexo init . $ npm install $  和 npm install hexo-deployer-git 到此，hexo安装完毕。浏览器输入本地，前面配置均正确的情况下，正常显示博客首页。

