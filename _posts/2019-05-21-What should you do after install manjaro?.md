---
layout:     post
title:      "What should you do after install manjaro?"
subtitle:   "notes of manjaro"
date:       2019-05-21 10:42:30
author:     "SHIELD-SKY"
header-img: "img/manjaro-kde.png"
tags:

- Linux

- Manjaro


---

## Change  mirrors

```shell
sudo pacman-mirrors -i -c China -m rank
```

Choose which mirror you want to use.

Refresh the cache:

```shell
sudo pacman -Syy 
```



## open-vm-tools

When you have installed manjaro on VMware Workstation,  open-vm-tool is installed.

To enable copy and paste between host and gust **gtkmm3** is required.

 Try to install **gtkmm3** manuslly if it does not work properly.



## Common pacman command

```shell
pacman -S  软件名   #安装instll
pacman -Syu    #更新update
pacman -R 软件名    #移除remove
```



## input method

```shell
sudo pacman -S fcitx-im
sudo pacman -S fcitx-configtool
sudo pacman -S fcitx-googlepinyin
```

- Add  configure file,  **sudo vim ~/.xprofile**, then reboot .

  

```shell
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

## yay

```shel
pacman -S yay
```

## zsh

```shell
sudo pacman -S zsh

sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

chsh -s /bin/zsh
```



##reference

- [最受欢迎Linux发行版, Manjaro折腾全记录(超长超详细)](https://mp.weixin.qq.com/s?__biz=Mzg3MDAyMDU2Ng==&mid=2247483750&idx=1&sn=064d079f5485eb350002c7db93d351cc&chksm=ce9565dff9e2ecc9195f28580b2d68b1c692f56e73f50513f798199307b10d7ad21d4503b6e3&scene=27&ascene=0&devicetype=android-25&version=2700043a&nettype=WIFI&abtest_cookie=BQABAAgACgALABIAEwAHAJ6GHgAjlx4AxpkeANyZHgDxmR4AAZoeAAOaHgAAAA%253D%253D&lang=en&pass_ticket=Rd0yBVHqxpj4qwvttbu7SPa47JgD49tqKcv3FPKPqyJBHACjcQIrpBDG%252BKsEvlE8&wx_header=1)
- [VMware/Installing Arch as a guest](https://wiki.archlinux.org/index.php/VMware/Installing_Arch_as_a_guest_(简体中文))

