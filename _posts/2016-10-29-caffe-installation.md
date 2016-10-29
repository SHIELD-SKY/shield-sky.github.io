---
layout:     post
title:      "caffe安装后续"
subtitle:   "给自己和学弟们留点笔记"
date:       2016-10-29 21:08:59
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- CUDA
- cudnn
- caffe
- deep learning
---

### 安装cuda8.0

下载CUDA需要注册和登陆NVIDIA开发者账号，[CUDA8下载页面](https://developer.nvidia.com/cuda-release-candidate-download)提供了很详细的系统选择和安装说明，

这里选择了Ubuntu14.04系统runfile安装方案，千万不要选择deb方案，前方无数坑.

```
可能需要事先 chmod +x cuda_8.0.27_linux.run 

./cuda_8.0.27_linux.run 
```

执行后会有一系列提示让你确认，非常非常非常非常关键的地方是是否安装361这个低版本的驱动：

Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 361.62?

答案必须是n，否则[之前](http://shield-sky.github.io/2016/10/29/nvidia-driver-installation/)安装的GTX960驱动就白费了，而且问题多多(可能会进不了桌面)。

安装完毕后，再声明一下环境变量，并将其写入到 ~/.bashrc 的尾部:

```
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

最后再来测试一下CUDA，运行：

```
nvidia-smi
```

结果如下所示：

```
Sat Oct 29 21:17:01 2016
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 367.57                 Driver Version: 367.57                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 970     On   | 0000:01:00.0      On |                  N/A |
| 34%   22C    P8    17W / 250W |    213MiB /  4028MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID  Type  Process name                               Usage      |
|=============================================================================|
|    0      1246    G   /usr/bin/X                                     150MiB |
|    0      2134    G   compiz                                          61MiB |
+-----------------------------------------------------------------------------+
```

再来试几个CUDA例子：

```
cd 1_Utilities/deviceQuery
make
./deviceQuery

cd ../../5_Simulations/nbody/
make
./nbody -benchmark -numbodies=256000 -device=0
```

### 安装cudnn

```
tar -zxf cudnn-8.0-linux-x64-v5.0-ga.tgz
cd cuda
sudo cp lib64/* /usr/local/cuda/lib64/
sudo cp include/cudnn.h /usr/local/cuda/include/
```

### 安装caffe
#### 各种依赖

```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libatlas-base-dev 
sudo apt-get install  python-dev 
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-dev libhdf5-serial-dev protobuf-compiler gfortran libjpeg62 libfreeimage-dev libatlas-base-dev git python-dev python-pip libgoogle-glog-dev libbz2-dev libxml2-dev libxslt-dev libffi-dev libssl-dev libgflags-dev liblmdb-dev python-yaml python-numpy
```

Then run:

```
sudo easy_install pillow
```

You could have "TypeError: 'NoneType' object is not callable" error when installing pillow, then try:

```
sudo apt-get install pypy-dev
```

Now we can download Caffe. Navigate to the directory of your choice for the cloning.

```
cd ~
git clone https://github.com/BVLC/caffe.git
```
We now install more dependencies. Warning: This takes 10-30 minutes.

```
cd caffe
cat python/requirements.txt | xargs -L 1 sudo pip install
```
Now we update the Makefile:

```
cp Makefile.config.example Makefile.config
vi Makefile.config
```

1. Uncomment the line: USE_CUDNN := 1
2. Make sure the CUDA_DIR correctly points to our CUDA installation.
3. If you want the Matlab wrapper, uncomment the appropriate MATLAB_DIR line.

Now we build Caffe. Set X to the number of CPU threads (or cores) on your machine. Use the command ***htop*** to check how many CPU threads you have.

```
make pycaffe -jX
make all -jX
make test -jX
```
Now to quickly test Caffe, from the CAFFE_ROOT (wherever the Caffe code resides)

```
./data/mnist/get_mnist.sh
./examples/mnist/create_mnist.sh
./examples/mnist/train_lenet.sh
```
You may get errors for create_mnist.sh but run train_lenet.sh anyway. Chances are it will still work. If you see the network training, then everything has been successfully set up.

If you want to use Python wrapper for caffe, then you should add path to the PYTHONPATH variable:

```
export PYTHONPATH=/home/username/caffe/python
我的caffe放在/root下：

export PYTHONPATH=/root/caffe/python

```

### 参考:

[深度学习主机环境配置: Ubuntu16.04+Nvidia GTX 1080+CUDA8.0](http://www.52nlp.cn/深度学习主机环境配置-ubuntu-16-04-nvidia-gtx-1080-cuda-8)

[http://caffe.berkeleyvision.org/installation](http://caffe.berkeleyvision.org/installation.html#compilation)

[Install Caffe on EC2 from scratch (Ubuntu, CUDA 7, cuDNN 3)](https://github.com/BVLC/caffe/wiki/Install-Caffe-on-EC2-from-scratch-(Ubuntu,-CUDA-7,-cuDNN-3))






