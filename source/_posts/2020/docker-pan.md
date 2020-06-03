---
title: docker-pan
permalink: docker-pan
date: 2020-06-03 21:22:25
tags: 网盘 aria
categories: work
---
[root@VM_0_11_centos ~]# cat /etc/docker/daemon.json 
{
"registry-mirrors": [
"https://mirror.ccs.tencentyun.com",
"https://registry.docker-cn.com",
"https://docker.mirrors.ustc.edu.cn",
"http://hub-mirror.c.163.com",
"https://cr.console.aliyun.com/"
]
}
systemctl restart docker

https://hub.docker.com/r/jaegerdocker/pan/
Docker:Filerun+AriaNg+Aria2,Personal cloud disk. 

docker run --name=pan -v /home/pxm/datapan:/var/www/html/system/data/default_home_folder  -dti -p 8081:80 -p 6800:6800 jaegerdocker/pan
#冒号":"前面的目录是宿主机目录，后面的目录是容器内目录。修改宿主机文件夹权限

Filerun：http://yourdomain.com:8081
登陆用户名：superuser，登陆密码：superuser。

AriaNg：http://yourdomain.com:8081/dweb
