---
title: kodexplorer
permalink: kodexplorer
date: 2020-06-09 23:07:28
tags: kodexplorer edit
categories: work
---

#Dockerfile
FROM  centos:7
ENV container docker
RUN curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS7-Base-163.repo
RUN sed -i 's#$releasever#7#g' /etc/yum.repos.d/CentOS-Base.repo
RUN sed -i 's#$basearch#x86_64#g' /etc/yum.repos.d/CentOS-Base.repo
RUN curl -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
RUN yum install curl  nginx net-tools php-cli php-fpm unzip php-gd php-mbstring -y
RUN sed -i "s#apache#nginx#g"  /etc/php-fpm.d/www.conf
RUN rm -f /etc/nginx/nginx.conf
ADD nginx.conf /etc/nginx/nginx.conf
RUN cd /usr/share/nginx/html  && \curl -o kodexplorer.zip http://static.kodcloud.com/update/download/kodexplorer4.37.zip && \unzip kodexplorer.zip

RUN chown -R nginx:nginx  /usr/share/nginx/html/
ADD init.sh /init.sh

CMD ["/bin/bash","/init.sh"]

#init.sh
#!/bin/bash
php-fpm -D 
nginx -g 'daemon off;'

#nginx.conf
#!/bin/bash
php-fpm -D 
nginx -g 'daemon off;'
[root@VM_0_11_centos kod]# cat nginx.conf 
worker_processes  5;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index index.php index.html index.htm;
        }
        location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
    }
}

################
docker build kod -t kod:v1

docker run --privileged=true -d  -p 8001:80  --name kod  kod:v1
