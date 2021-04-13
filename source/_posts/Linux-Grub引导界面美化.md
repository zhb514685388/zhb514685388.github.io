---
title: Linux Grub引导界面美化
date: 2021-04-14 02:23:07
tags:
-linux 
-KDE
-Manjaro
categories: linux
description:  主要是Linux Grub引导界面的美化
---

### 一.选择主题下载 


网址:[https://www.gnome-look.org](https://www.gnome-look.org)

在左边的菜单栏选择GRUB Themes，即右边内容处便刷新grub主题列表，默认是最新的主题列表。

这里下载的是vimix-1080p.tar.xz

### 二.安装主题
#### 1.对文件解压

      tar -jxf vimix -1080p.tar.xz


#### 2.放置文件主题
      sudo cp -r Vimix /usr/share/grub/themes/
#### 3.修改系统配置
      sudo vim /etc/default/grub
 找到
       
       #GRUB_THEME=
这是系统的主题设置，默认是注释的。我们取消注释（删掉#），然后修改等于号后面的路径，指向我们自定的主题文件的theme.txt

      GRUB_THEME="/usr/share/grub/themes/vimix/theme.txt"
>这里一定要注意是指向了theme.txt

### 三.重建镜像
      sudo grub-mkconfig -o /boot/grub/grub.cfg
>重新生成grub.cfg引导配置文件

首先查看 /etc/mkinitcpio.d 目录下的文件名，在终端输入：
      
      ls /etc/mkinitcpio.d

我的电脑上输出为linux510.preset,则在终端输入：
      
      mkinitcipo -p linux510

>重建initrd镜像

# 最后重启电脑