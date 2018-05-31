---
layout:     post
title:      "杂谈：关于Linux下安装调试代码"
subtitle:   "Linux如何让别人的代码跑起来的经验之谈"
date:       2018-05-31 11:03:30
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- Linux
---

> 最近师姐搞微分流形，想跑通张苗苗博士的[FlashC](https://bitbucket.org/FlashC/flashc/overview)看看效果，师姐和留学生经验略少，前些日子陪老外去了趟西安开会，闲来无事，突然想起写一篇经验之谈。

> 感谢[学弟](https://wfly1998.github.io)补充修改

#### 1.关于代码的获取

有人说，这还用问？下载呗。但是看到师姐和老外上来就犯了一个错误，因此费了好多时间。比如像bitbucket上的代码，有https和ssh选项，师姐和老外打开时，默认都是ssh，当他们想`git clone`时，一直因没有ssh证书而无法下载，这里可以选用https，直接`git clone`即可。

#### 2.获得代码后。。。

先看README.md，或者找找其他类似的wiki资料，看看有什么教程，并严格按照教程执行。

#### 3.关于各种依赖包

像FlashC，虽说文档中说需要两个依赖包，但是这两个依赖包依赖更多的依赖项。须一一安装。

#### 4.关于cmake make

安装包有几种途径：

(1) 使用`apt install` 
新安装的系统最好先改一下更新源，比如改成[清华的源](https://mirrors.tuna.tsinghua.edu.cn)会快很多，清华源可以在系统更新选项的设置里直接选择。然后`sudo apt update`一下用来获取软件源里所有库的信息。

一般编译时会提示缺少啥依赖库，可以搜一下有没有现成的可以直接`sudo apt install lib~` （一般这么装的库，都叫lib+库的名字）

(2) 使用ppa方法，这个我[以前](http://shield-sky.github.io/2016/10/29/nvidia-driver-installation/)提到过。

(3) 从源代码编译安装

 一般源代码下载完，看完README，看看有没有configure文件或者CMakeList.txt，建个build文件夹， 进入build，`../configure`或者`cmake ..`一下，然后再`make`， 再`make install`，install完之后 注意一下安装的位置，他会显示，以后可能会用到，如果不是安装到默认位置，可能还得修改环境变量。

```bash
    mkdir build
    cd build
    cmake ..
    make -jx  # x是指你的cpu的线程数，可以使用htop查看
    sudo make install
```

另外，**一定要注意需要库的版本**！！！`apt-get install`的时候可以选择版本号，如果没有的话自行搜索源码然后下载编译。
    
#### 5.其他诡异问题
见到错误先仔细查看shell中出现的错误提示，然后使用Google搜索一下错误内容，发散思维，联系蛛丝马迹。另外如何使用Google，简单点的方式就是用[Opera](https://blog.csdn.net/i_____miss__you/article/details/80105495)了，基本可以满足日常查资料了。


