---
title: first
permalink: first
date: 2022-12-16 22:46:45
tags:
categories:

---

update blog in 2022 year by docsify and visit it with url xxx/# . 

see afanda2 in claim.

将容器镜像从系统盘移到D盘
https://www.jianshu.com/p/dfbb3e9ecf8a

```
wsl --list --running
wsl --help
wsl --list -v
docker ps -q -a
docker rm  -fd816f3edfbd3 f55e7b1d69b9 b9ade05c14a4 6b497adc6da5 fb8f74ad134d 6dd2c86b
1d89e20e47 09b754c8f188 45ff8c10170a
docker rm  -f d816f3edfbd3 f55e7b1d69b9 b9ade05c14a4 6b497adc6da5 fb8f74ad134d 6dd2c86
d1d89e20e47 09b754c8f188 45ff8c10170a
docker ps -a
wsl --shutdown
wsl --export docker-desktop-data D:\winDocker\docker-desktop-data.tar
wsl --export docker-desktop D:\winDocker\docker-desktop.tar
wsl --unregister docker-desktop-data
wsl --unregister docker-desktop
wsl --import docker-desktop-data D:\winDocker\wsl\docker-desktop-data D:\winDocker\doc
ktop-data.tar
wsl --import docker-desktop D:\winDocker\wsl\docker-desktop D:\winDocker\docker-deskto
wsl --list --verbose
```

可以考虑使用 chocolatey 安装 kind
在WSL中使用kind创建kubernetes集群
https://zhuanlan.zhihu.com/p/426227999

下载ubuntu镜像，搭建本地环境

```docker
docker run -itd --name ubuntu-local ubuntu:18.04
echo 'deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse' >> sources.list
echo "deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse" >> sources.list
echo "deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse" >> sources.list
echo "deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse" >> sources.list
echo "deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse" >> sources.list
echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse" >> sources.list
echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse" >> sources.list
echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse" >> sources.list
echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse" >> sources.list
echo "deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse" >> sources.list
apt install vim-gtk
apt install gcc-7 golang-1.18* python3.8
```

#commit以上内容到镜像中，打开powershell，执行命令wsl，可进入新的shell提示符，其路径映射关系：
windows    wsl2      K8S-docker
D://       /mnt/d    /run/desktop/mnt/host/d

执行下面的创建命令：
docker run -itd --name ubuntu -v /d/winDocker/mount:/home/pxm/mount -p3000:3000 ubuntu:18.04_my

安装docsify，参考：https://cloud.tencent.com/developer/article/2086817
访问地址： https://pingzha.github.io/#/
