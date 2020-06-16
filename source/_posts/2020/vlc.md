---
title: vlc
permalink: vlc
date: 2020-06-16 22:36:15
tags: vlc rtmp
categories: work
---

安装 vlc-2.2.6.tar.gz
依赖：
qt-devel
lua-devel
gstreamer-devel
gstreamer-devel1
alsa-lib-devel
libgcrypt-devel
nasm

手动安装：
libmad-0.15.1b.tar.gz
./configure;make;make install
ffmpeg-2.7.7.tar.gz
./configure --enable-shared;make;make install

PKG_CONFIG_PATH=/usr/local/lib/pkgconfig/ ./configure --disable-a52 --enable-run-as-root
make;make install

xhost +
因为增加了编译选型 --enable-run-as-root，可以使用root用户打开vlc

使用docker启动软件
docker run -v /etc:/etc -v /tmp:/tmp -v /lib:/lib -v /usr:/usr -v /lib64:/lib64 -v/bin:/bin -v/home:/home --privileged -e DISPLAY=unix:0 -e GDK_SCALE -e GDK_DPI_SCALE -e LANG=zh_CN.UTF-8 --name vlc --cpuset-cpus=0-3 --memory-reservation=4G --net=host blank:latest /usr/local/bin/vlc --open file:///home/pxm/Wildlife.wmv

安装插件 libsrpos_plugin-0.5，可支持播放记录保存功能
./configure;make;make install

考虑可以手动触发记录保存，快捷键 “r”
在开源代码中做修改：
Open函数中： var_AddCallback( pl_Get(p_intf), "random", &ItemChange, p_intf);
Close函数中：var_DelCallback( pl_Get(p_intf), "random", &ItemChange, p_intf);
新增函数： static void TimeRecord(vlc_object_t *p_this);
ItemChange函数中： TimeRecord(p_intf);

############################################
流媒体服务搭建
使用nginx-1.9.10编译
./configure --add-module=../nginx-rtmp-module

在nginx.conf中增加配置：
rtmp {
  server {
    listen 1935;
    chunk_size 4096;
    application live {
      live on;
      record off;
    }
  }
}

推流：
ffmpeg -re -i /home/pxm/Wildlife.wmv -f flv rtmp://localhost:1935/live/room

拉流：
vlc 启动输入地址： rtmp://localhost:1935/live/room
