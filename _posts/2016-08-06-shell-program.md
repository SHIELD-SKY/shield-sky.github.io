---
layout:     post
title:      "UNIX学习笔记"
subtitle:   "Bourne shell编程"
date:       2016-08-06 11:01:45 
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- UNIX
- shell
---
- 数值型数据的处理 

```
expr args
```
- here 文档

```
command << [-] input_marker
... input data ...
input_marker
```
- 中断（信号）处理

```
trap ['command-list'][signal-list]
```
- exec命令与文件输入输出

```
exec command
```
- Bourne shell函数
- 调试shell程序

