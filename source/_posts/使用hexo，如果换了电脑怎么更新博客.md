---
title: 使用hexo，如果换了电脑怎么更新博客
date: 2018-09-13 21:17:07
tags:
---


本地资料丢失后的流程当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：

  	git clone -b hexo git@github.com:zhb514685388/zhb514685388.github.io.git拷贝仓库（默认分支为hexo）；

在本地新拷贝的hb514685388.github.io文件夹下通过Git bash依次执行下列指令：

	npm install hexo、
	npm install、
	npm install hexo-deployer-git  扩展插件帮助连接仓库
	（记得，不需要hexo init这条指令）
