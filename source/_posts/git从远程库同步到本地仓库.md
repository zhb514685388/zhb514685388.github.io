---
title: git从远程库同步到本地仓库
date: 2018-09-13 21:38:19
tags:
---

## 远程仓库发生改变，本地仓库没有改变
- #### 首先，查看远程仓库 git remote -v


		1  $ git remote -v
		2   origin	git@github.com:{User}/Understanding_Unix-Linux_Programming.git (fetch)
		3   origin	git@github.com:{User}/Understanding_Unix-Linux_Programming.git (push)

- #### 把远程库更新到本地 git fetch origin master
<!--more-->


		1 $ git fetch origin master
		2 Warning: Permanently added the RSA host key for IP address '{IP address such as: 192.168.1.1 }' to the list of known hosts.
		3 From github.com:{User}/Understanding_Unix-Linux_Programmin
	    4 branch            master     -> FETCH_HEAD

- #### 比较远程更新和本地版本库的差异 git log master.. origin/master


		1 $ git log master.. origin/master
		2 commit ce39f8b3eeee898a2a038444f897f2aef3673493
		3 Author: {User} <794870409@qq.com>
		4 Date:   Fri Feb 26 14:14:39 2016 +0800
		5
	    6 {The context origin added ... }

- #### 合并远程库 git merge origin/master

- 有差异



		1 $ git merge origin/master
		2 Updating eb32b20..ce39f8b
		3 Fast-forward
	    4  README.md | 2 +-
		5  1 file changed, 1 insertion(+), 1 deletion(-)



- 无差异


		1 $ git merge origin/master
		2 Already up-to-date
