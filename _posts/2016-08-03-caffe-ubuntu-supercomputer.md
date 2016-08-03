---
layout:     post
title:      "倚天超级计算机折腾笔记"
subtitle:   "终于来好设备了"
date:       2016-08-3 
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- DeepLearning
---

## 组RAID
进BIOS 用legncy模式，保存设定.


进入ubuntu 14.04安装程序
Crtrl+Alt+F2 进入U盘 dpkg -i 安装RAID 14.04的驱动

下载[cuda](https://developer.nvidia.com/cuda-downloads)

1.http://caffe.berkeleyvision.org/install_apt.html

2.https://github.com/BVLC/caffe/wiki/Install-Caffe-on-EC2-from-scratch-(Ubuntu,-CUDA-7,-cuDNN-3)



```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
```
装ATLAS
```
sudo apt-get install libatlas-base-dev
```

```
sudo apt-get install the python-dev
```

```
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```

```
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install build-essential
```

把下好的cuda复制到桌面


```
cd Desktoop/
chmod +x cuda_7.0.28_linux.run
mkdir nvidia_installers
./cuda_7.0.28_linux.run -extract=`pwd`/nvidia_installers
```

此时发现无法run，需关闭图形界面，在命令行下操作

由于各种原因，这篇文章暂时先写这些，重点是组RAID，然后就是RAID的驱动，其他的caffe相关配置和平常电脑一样，但是至今安装完英伟达驱动后，超算的图形界面就无法使用的问题一直未解决，希望高手可以相告解决方案，显卡是K40



