---
title: docker_centos
permalink: docker_centos
date: 2020-05-31 22:33:19
tags: docker
categories: work
---

```
docker pull docker.io/simonpeng/centos:latest

#/opt/start.sh
docker run -itd --hostname=docker_cloud --net=host --privileged --name=test -v/opt:/opt docker.io/simonpeng/centos 
supervisord -n -c /etc/supervisord.conf

#telnet 127.0.0.1

docker commit test
docker push docker.io/simonpeng/centos:latest

docker save test > test.tar
docker load < test.tar
```
