---
title: deepin下搭建基于github和hexo的个人博客
date: 2018-09-13 21:11:19
tags:
---

### 搭建步骤：
---

[1.安装git](#1)

[2.安装Node.js及npm](#2)

[3.安装hexo](#3)

[4.在github上创建并设置远程库](#4)

[5.修改内容](#5)

---


<span id="1">１. 安装git</span>


		$ sudo apt-get install git 

- 查看git版本


		$ git version 
<!--more-->

<span id="2">２． 安装Node.js及npm</span> 

- 可以直接命令安装,但是命令安装的不是最新版本。


		$ sudo apt-get install nodejs $ sudo apt-get install npm 

- 本博客采用第二种方法，首先官网下载最新版，然后解压。将node,npm命令设置全局命令,下载网址：https://nodejs.org/zh-cn/


		$ sudo ln -s /home/hb/Downloads/node-v8.6.0-linux-x64/bin/node /usr/local/bin/node 
		$ sudo ln -s /home/hb/Downloads/node-v8.6.0-linux-x64/bin/npm /usr/local/bin/npm 

- 查看版本：


		$ node -v $ npm -v 

 <span id="3">3、 安装hexo</span>

	$ npm install -g hexo-cli 


   安装路径/home/hb/Downloads/node-v8.6.0-linux-x64/lib/node_modules/hexo-cli，此时输入命令hexo会提示“未找到命令”，此时要将hexo-cli/bin/文件夹下的hexo命令设置为全局：

	$ sudo ln -s /home/hb/Downloads/node-v8.6.0-linux-x64/lib/node_modules/hexo-cli/bin/hexo /usr/local/bin/hexo 
    
   再输入hexo命令可以正常显示。 创建一个空文件夹，此处名为hexo：
	
	$ mkdir hexo 
	$ cd hexo 
	$ hexo init . 
	$ npm install 
	$ npm install hexo-deployer-git 

 到此，hexo安装完毕。浏览器输入本地，前面配置均正确的情况下，正常显示博客首页。
 
 
  <span id="4"> 4、在github上创建并设置远程库</span>
  
  <span id="5">5、修改内容</span>
  
   打开_config.yml
   
    1 #site
    2 title: 网站标题
    3 subtitle: 副标题
    4 description: 个人签名
    5 author: 姓名
	6 language: zh-Hans
	7 timezone:
   注：所有的配置“:”符号后面都要带空格，否则执行本地测试直接失败
   
   配置自己的远程仓库地址
   
    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repo: https://github.com/zhb514685388/zhb514685388.github.io.git
      branch: master

 6、布署到博客上
 	
    1、 hexo new "postName" #新建文章输入内容
    2、 hexo clean 清空缓存
    3、 hexo g
    4、 hexo d
    5、 git add .-->git commit -m "  " 
如果是第一次使用git，在使用git的时候会提示输入用户名和密码，用户名是自己的注册邮箱。 

	git remote add origin https://github.com/zhb514685388/zhb514685388.github.io.git 
    
    
 推送到远程仓库（github） 
这里建议创建一个新的分支hexo，用于管理hexo文件。提交的的时候只提交hexo网站html、css、等源文件。而默认的master用来部署更新项目

创建并切换到新建分支： 
	
    git checkout -b hexo 
    
将分支推送到远程仓库： 

	git push origin hexo 
    git push orgin hexo //备份
    hexo s
 
 

