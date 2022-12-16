---
title: hexo
permalink: hexo
date: 2020-05-31 22:45:14
tags: hexo build
categories: work
---

yum -y install git
npm install hexo-cli --save
npm install hexo-deployer-git --save
npm install hexo-generator-searchdb --save

hexo init web

#先调整github.io的分支为hexo，不要用master
git clone git@github.com:pingzha/pingzha.github.io.git
#下载完成后再通过界面修改默认分支为master

#下载next主题，使用Gemini模板
git clone https://github.com/theme-next/hexo-theme-next themes/next

#_config.yml
theme: next
search:
  path: search.xml
  field: post
  format: html
  limit: 10000

#theme/next/_config.yml
local_search:
  enable: true

使用命令发布博客 
hexo new post xxx

使用批处理脚本auto.sh上传git保存
git add --all .
git commit -m "add blog"
git push

hexo g -d
exit
