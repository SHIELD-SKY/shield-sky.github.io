---
layout:     post
title:      "caffe安装之ubuntu下NVIDIA驱动安装"
subtitle:   "给自己和学弟们留点笔记"
date:       2016-10-29 15:59:59
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- NVIDIA
- caffe
- deep learning
---

>孔子曰：唯女子与小人为难养也，近之则不孙，远之则怨. 
>
>纪晓岚曰：此乃男女相恋之术也。（最近看舍友重温《铁齿铜牙纪晓岚》）

### 前提
1. ubuntu 14.04 （必须是这个版本，别的版本我不敢保证，之前试过高版本的，所谓的向下兼容还是会有各种问题！）
2. [ubuntu root账户登陆](http://jingyan.baidu.com/article/27fa73268144f346f8271f83.html )
3. [ssh远程登陆服务器安装和配置](http://jingyan.baidu.com/article/9c69d48fb9fd7b13c8024e6b.html)

 2，3 可选，我只是比较习惯这样罢了

### 安装NVIDIA驱动

关于ubuntu软件源与ppa源的常识课参见[这里](http://blog.mythsman.com/?p=2043)

之前安装是从官网下载的.run文件，然后经过很繁琐的步骤手动安装，貌似是每因为ubuntu内核升级的原因，每次upgrade和dist-upgrade的之后ubuntu就无法登陆图形化界面了，关于这个问题的解决方案我找到一个，但是没尝试，需要的人参考[这个](http://forum.ubuntu.org.cn/viewtopic.php?f=42&t=141431)吧.至于一开始有人装完驱动就无法进入图形界面，我猜想原因之一是英伟达显卡驱动的bug，因为我在英伟达官网的最新驱动的[介绍](http://www.geforce.cn/drivers/results/108769)中发现貌似是修复了一些bug。

#### 一、PPA方法

```
sudo apt-get update
sudo apt-get upgrade
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-367
sudo apt-get install mesa-common-dev
sudo apt-get install freeglut3-dev
reboot
```

添加ppa后，我update后，显示hash校验和不符的问题，后来我把实验室的路由器里边的ss调整为全局走代理，你懂得。然后就可以正常update了。

#### 二、官方下载驱动

##### 编译依赖

```
sudo apt-get install build-essential pkg-config xserver-xorg-dev linux-headers-`uname -r`
```

##### 屏蔽开源驱动nouveau

其实可以跳过，在安装过程那一部，运行驱动安装程序，会问您要不要帮您屏蔽，选择yes退出后重启即可。 下面两种方案只能采取一种。

1. blacklist.conf法

	```
	sudo nano /etc/modprobe.d/blacklist.conf
	```
	
	添加

	```
	blacklist vga16fb

	blacklist nouveau

	blacklist rivafb

	blacklist nvidiafb

	blacklist rivatv
	```

2. 也可以通过Grub2屏蔽

   ```
   sudo nano /etc/default/grub
  
   GRUB_CMDLINE_LINUX="nomodeset"  #修改这行
   
   sudo update-grub
   ```
   
##### 注销系统，关闭图形环境
 
```
 sudo /etc/init.d/kdm stop  #适用于Kubuntu
 
 sudo /etc/init.d/gdm stop  #适用于Ubuntu
 
 sudo stop lightdm  #适用于Ubuntu(11.10)
```
 
##### 安装过程

```
cd /home/用户名

sudo sh NVIDIA-Linux-x86-185.18.14-pkg1.run 

或者
sudo sh NVIDIA*.run 
```

安装过程中

如果提示有旧驱动，询问是否删除旧驱动，选Yes；

如果提示缺少某某模块（modules），询问是否上网下载，选no；

如果提示编译模块，询问是否进行编译，选ok；

如果提示将要修改Xorg.conf，询问是否允许，选Yes；

##### 启动图形环境

```
sudo /etc/init.d/kdm restart  #适用于Kubuntu

sudo /etc/init.d/gdm restart #适用于Ubuntu

sudo start lightdm #适用于Ubuntu(11.10)
```




