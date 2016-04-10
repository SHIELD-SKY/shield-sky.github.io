---
layout:     post
title:      "pbc library on ubuntu"
subtitle:   "安装笔记"
date:       2016-04-10
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
    - pbc library
---

昨天被一学长问起pbc的配置以及跑demo，结果搞了很长时间才想起怎么搞，还是做个笔记保险啊！

*pbc library on mac* 请看我另一篇[博客](http://blog.csdn.net/shield_sky/article/details/50528926)



工具/原料
==

- ubuntu系统
- pbc library、GMP Library、M4、flex、bison


方法/步骤
==

### 1.安装Pbc library依赖的库：		
M4、flex、bison 其中在ununtu系统terminal中 M4、flex、bison均可以通过apt-get install方式安装。在Linux系统中键入如下命令即可安装相应的包。

``` 
  apt-get install M4 		  	
  apt-get install flex 	
  apt-get install bison
```

### 2.安装GMP库：	
GMP库下载地址如下：https://gmplib.org/ 
下载并解压，在terminal里进入解压后的文件夹进行安装，方法如下：

```./configure 	
make 	
make check 	
make install	
```

### 3.Pbc library库安装 :		
pbc（The Pairing-Based Cryptography Library）下载地址如下：[http://crypto.stanford.edu/pbc/download.html](http://crypto.stanford.edu/pbc/download.html)	
 下载并解压，在terminal里进入解压文件夹安装，方法如下：
  
```
./configure 
make 	
make install 	
```
至此pbc library 在ubuntu系统中的安装完成。接下来就是验证pbc 库是否安装正确

### 4.验证
我在example下用hess.c文件试了一下 运行:

```
gcc -o test333 hest.c -lpbc -lgmp -I /usr/local/  include/pbc /usr/local/include/gmp.h 
```
 然后返回上一级:
   
```
 cd ../
```
 运行:
 
```
  ./example/test333 param/a.param
```
  		
  如果你看到如下类似输出，恭喜pbc安装成功了
  
![](/img/pbc_shiyan.jpg)