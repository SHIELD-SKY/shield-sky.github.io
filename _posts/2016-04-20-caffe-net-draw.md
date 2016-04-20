---
layout:     post
title:      "draw_net.py绘制caffe net结构"
subtitle:   "caffe大法好"
date:       2016-04-20 
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- caffe
- deep learning
---

环境
==
- ubuntu
- pycaffe

步骤
==

### 1.安装pydot(可使用pip list命令 查看是否已安装)

```
pip install pydot
```

### 2.安装graphviz
	
```
sudo apt-get install graphviz
```
### 3.绘图
进入caffe-master/python目录下运行：
	
```
python draw_net.py ../examples/cifar10/	cifar10_quick_train_test.prototxt cifa10.png
```

### 4.结果
![](/img/cifa.jpg)