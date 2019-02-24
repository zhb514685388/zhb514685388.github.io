---
title: hexo命令
date: 2019-02-24 09:04:34
tags:
---
    hexo new "postName" #新建文章
    hexo new page "pageName" #新建页面
    
<!--more-->

    hexo generate  生成静态页面至public目录
    hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
    hexo deploy #部署到GitHub
    hexo help  # 查看帮助
    hexo version  #查看Hexo的版本



默认情况下，生成的博文目录会显示全部的文章内容，如何设置文章摘要的长度呢？
答案是在合适的位置加上 !--more-->


发布文章步骤：

    1、hexo new "postName" #新建文章输入内容
    2、hexo clean 清空缓存
    3、hexo g
    4、hexo d
    5、git add .-->git commit -m "" -->git push orgin hexo //备份
    6、hexo s