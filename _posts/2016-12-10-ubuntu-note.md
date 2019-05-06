---
layout:     post
title:      "Ubuntu下添加开机自动启动脚本"
subtitle:   "vps操作杂记"
date:       2016-12-10 13:58:59
author:     "SHIELD-SKY"
header-img: "img/post-bg-2015.jpg"
tags:
- Linux
---

>最近vps被google学术禁了，只能换个ip了，突然想写个自启动脚本，以后懒得重启后，手动开服务了

~~推荐[vultr](http://www.vultr.com/?ref=6913433)的vps，实测用日本的最快（昨天我我刚试了洛杉矶，新加坡，日本），用俺的链接俺也有反馈，哈哈！~~

更新：vultr早已被玩烂了，推荐[onevps](https://www.onevps.com/portal/aff.php?aff=1076)，感觉客服很不错，回复特别快，日本线路速度也超好

## 正题

2018年5月31日更新

###新方法：使用`Systemd`

关于`Systemd`的历史和基本知识可以学习一下[阮一峰](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)的文章

在此我只记录一下自己用的脚本

```bash
vim /usr/local/bin/run_start_ssrr.sh

	#!/bin/bash
	cd ~
	cd shadowsocksr/shadowsocks/
	python3 server.py -d start
	
vim /usr/local/bin/run_stop_ssrr.sh
	
	#!/bin/bash
	cd ~
	cd shadowsocksr/shadowsocks/
	python3 server.py -d stop

cd /usr/local/bin/

chmod +x *

vim /etc/systemd/system/run_ssrr.service

[Unit]
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/run_start_ssrr.sh
ExecStop=/usr/local/bin/run_stop_ssrr.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target

chmod 664 /etc/systemd/system/run_ssrr.service

systemctl daemon-reload 

systemctl enable run_ssrr.service

systemctl start  run_ssrr.service

systemctl status run_ssrr.service

```
----
### 方法一：编辑rc.local脚本

**rc.local**脚本是一个ubuntu开机后会自动执行的脚本，我们可以在该脚本内添加命令行指令。
该脚本位于**/etc/**路径下，需要**root**权限才能修改。

```bash
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

exit 0
```

>注意: 一定要将命令添加在 exit 0之前



### 方法二：添加一个开机启动脚本

上面的方法虽然奏效，但是将所有不同的脚本指令写入同一个文件不是一个好的style。我们可以自己写一个run.sh,然后让系统在开机时自动执行。

#### 1.建立自己的脚本

首先我们需要写一个需要执行的脚本，比如这次我想让我的vps开机启动kcp服务

```bash
#!/bin/bash
cd ~/kcp/
./kcp-start.sh
```

随后将脚本保存为**run_server.sh**

#### 2.修改脚本权限

```bash
sudo chmod +x run_server.sh
```

#### 3.将脚本放置在启动路径下

将**run_server.sh**移动到**/etc/init.d**路径下，可以直接拷贝，也可以链接过去

```bash
sudo cp run_server.sh /etc/init.d/
```

#### 4.将脚本添加到启动脚本

执行如下指令，在这里**90**表明一个优先级，越高表示执行的越晚

```bash
cd /etc/init.d/
$ sudo update-rc.d run_server defaults 90
```

#### 5.如何移除该脚本

```bash
sudo update-rc.d -f run_server.sh remove
```


## 参考

[Ubuntu下添加开机自动脚本](http://jackqdyulei.github.io/2016/03/06/linux-auto-script/)

