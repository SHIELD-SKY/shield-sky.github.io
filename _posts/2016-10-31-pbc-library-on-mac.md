---
layout:     post
title:      "pbc library on mac"
subtitle:   "基于对的加密"
date:       2016-10-31 15:47:59
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- pbc library
---

mac 上安装pbc library 非常简单只需一条命令

```
brew install pbc
```

之后 在xcode中新建工程，右键工程Add files to “project name”.. 

敲一下 “/”键，输入 gmp和pbc的安装位置

```
/usr/local/Cellar/
```

回车后 把gmp和pbc下lib文件夹里的所有文件添加。 
然后在project下含main.c文件夹下的，添加

```
/usr/local/Cellar/
```

路径下的gmp和pbc下include文件夹的所有文件。
修改project的Build Settings下的Search Paths的Header Search Paths 和Library Search Paths 
Header Search Paths下添加：

```
/usr/local/Cellar/0.5.14/inlcude/pbc
/usr/local/Cellar/gmp/6.0.0a/include
```

Library Search Paths下添加：

```
/usr/local/Cellar/pbc/0.5.14/lib
/usr/local/Cellar/gmp/6.0.0a/lib
```

然后就可以写程序了。

点击xcode下的perferences之后点击locations，点击Advanced选择Custom，选择Absolute,然后改一下路径，我是把路径设置到了桌面上，然后桌面上就会出现一个Build文件夹，在Products/debug下，就是生成的可执行程序，可将param文件放在此文件夹下，如果想在调试的时候 调用类似a.param的参数文件，先按住option键，然后点运行，会弹出窗口，惦记左侧run，在Arguments Passed On Launch下 添加文件名即可。